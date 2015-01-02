---
layout: post
title: "解决Xerces-C XML parser中EnitiyResolver不起作用的问题"
comments: true
date: 2004-04-30 01:32
updated: 2012-06-18 23:48
tags:
- SoftwareDev
---
在SAX和DOM两种paser API中，均提供了EntityResolver接口，应用程序可通过实现这个接口的resolveEntity()方法来实现对外部实体访问的控制，例如将对网络URL的访问重定向为对本地文件或者数据库的访问。Xerces软件包提供了例子redirect来演示这个功能。

在我们的程序中，parse VXML文件时需要使用外部的schema，而这些schema又会间接引用其他的schema或者DTD，这些外部实体都是通过URL形式的system id来指定的（基本上都是在[www.w3c.org](http://www.w3c.org)上）。考虑到程序运行环境不一定与internet连接，还有远程网络的效率也不高，我们在程序中实现了自己的EntityResolver，把所有对这些schemas、DTDs的访问都截取下来，返回程序中内置的拷贝。
    
``` c++
    class DTDResolver : public EntityResolver {
    public:
        virtual InputSource * resolveEntity(const XMLCh * const publicId,
            const XMLCh * const systemId)
        {
            if (Compare(systemId, L"http://www.w3.org/TR/voicexml20/vxml.xsd")) {
                VXIcharToXMLCh name(L"http://www.w3.org/TR/voicexml20/vxml.xsd (SB)");
                return new MemBufInputSource(VALIDATOR_DATA + VALIDATOR_VXML,
                    VALIDATOR_VXML_SIZE,
                    name.c_str(), false);
            }
    
            if (Compare(systemId, L"http://www.w3.org/2001/xml.xsd")) {
                VXIcharToXMLCh name(L"http://www.w3.org/2001/xml.xsd");
                return new MemBufInputSource(VALIDATOR_DATA + VALIDATOR_XML,
                    VALIDATOR_XML_SIZE,
                    name.c_str(), false);
            }
    
            if (Compare(systemId, L"XMLSchema.dtd")) {
                VXIcharToXMLCh name(L"XMLSchema.dtd");
                return new MemBufInputSource(VALIDATOR_DATA + VALIDATOR_SCHEMA_DTD,
                    VALIDATOR_SCHEMA_DTD_SIZE,
                    name.c_str(), false);
            }
    
            // ......
    
            return NULL;
        }
    };
    
    // ......
    
    parser = XMLReaderFactory::createXMLReader();
    DTDResolver * dtd = new DTDResolver();
    parser->setEntityResolver(dtd);
```

在测试中发现，对某些外部实体的访问成功截取了，但是另外一些却不成功，parser依然尝试通过网络访问，然后由于网络不通而访问失败，造成xml文件无法parse。

对程序进行跟踪，发现了问题出现的规律：所有对schema文件的引用处理都正常，无论是include还是import方式，也无论是多少层的间接引用；在被parse的XML文件中DOCTYPE声明的外部实体引用，处理也正常，故此xerces自带的redirect例子是不会出问题的；但是，schema文件的DOCTYPE声明的外部实体引用就不能被正确处理了，根本不会调用指定的resolveEntity()方法。

这时，基本上可以确认问题是出在xerces身上，而不是我们的程序的问题了。于是仔细跟踪xerces代码对schema parse的执行过程，最终发现了问题。

XercesDOMParser类有两个resolveEnitiy方法：
    
``` c++
    InputSource* XercesDOMParser::resolveEntity(const XMLCh* const publicId,
                                                const XMLCh* const systemId,
                                                const XMLCh* const baseURI)
    {
        if (fEntityResolver)
            return fEntityResolver->resolveEntity(publicId, systemId);
        return 0;
    }
    InputSource* XercesDOMParser::resolveEntity(XMLResourceIdentifier* resourceIdentifier)
    {
        if (fEntityResolver)
            return fEntityResolver->resolveEntity(resourceIdentifier->getPublicId(),
                                                    resourceIdentifier->getSystemId());
        if (fXMLEntityResolver)
            return fXMLEntityResolver->resolveEntity(resourceIdentifier);
        return 0;
    }
```

根据代码注释，前者是deprecated的，被后者代替。

由XercesDOMParser派生出XSDDOMParser类，这个类是专门用来parser xsd文件（也就是schema）的。这个类重载了XercesDOMParser的三个参数的resolveEntity方法，用fUserEntityHandler代替fEntityResolver的方法调用。
    
``` c++
    InputSource* XSDDOMParser::resolveEntity(const XMLCh* const publicId,
                                             const XMLCh* const systemId,
                                             const XMLCh* const baseURI)
    {
        if (fUserEntityHandler)
            return fUserEntityHandler->resolveEntity(publicId, systemId, baseURI);
    
        return 0;
    }
```

但是XSDDOMParser没有重载第二个resolveEntity方法，估计是第二个方法是后来才加入XercesDOMParser的，在XSDDOMParser中没有同步修改。因此，依然是使用fEntityResolver，而不是fUserEntityHandler，就造成了没有调用到我们定义的resolveEntity方法，依然按照原来的system id去访问网络了。

问题找出来，解决就简单了，在XSDDOMParser中把另外一个resolveEntity方法也重载了，改为使用fUserEntityHandler。修改之后，测试通过。
    
``` c++
    InputSource* XSDDOMParser::resolveEntity(XMLResourceIdentifier* resourceIdentifier)
    {
        if (fUserEntityHandler)
            return fUserEntityHandler->resolveEntity(resourceIdentifier);
    
        return 0;
    }
```

这个问题在Xerces-C 2.4和2.5版本都存在，IBM的XML4C与Xerces-C是影子项目，也有同样的问题。
