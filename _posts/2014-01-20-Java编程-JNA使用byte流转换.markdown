---
layout: post
title: Java编程-JNA使用byte流转换
categories: [java编程]
description: Java编程-JNA使用byte流转换
keywords: java编程
---
<body><div id='preview-contents' class='note-content'>



<p>最近因为一个项目，改造客户现有系统，因为客户现存计费系统和J2EE化的系统之间界限定义不是很好，为了保证项目在不重构的情况下，很好的优化J2EE化系统，利用Java多线程优势来改造批量计算扣费的优化。没办法，将核心的计算库保留原有c++应用动态库，多线程应用方面采用java这样一个混搭的模式。</p>

<p>这个过程比较的痛苦，其中具体的改造思路涉及业务层面的就不多描述了，主要还是分享下java到c++核心库之间的数据传递的一个简单做法，当然如果有更好的处理技巧，熟悉这方面的专家也可以给我一点意见；</p>

<p><strong>场景如下：</strong></p>



<p><img longdesc="./JNA1.png" alt="Alt text" title="" type="image/png" src="data:image/png;base64,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" class=""></p>

<p>1、c++核心库，主要负责计算销帐逻辑，输入的数据有账本和账单，账本可以理解为资金，但是是分列式流水记录，账单是用户的消费记录，记录用户消费各类业务的账单；（核心库负责通过销帐的规则，计算账本和账单，最后得出扣减的记录，供结余变化参考。）</p>

<p>2、外围独立模块采用java应用，发挥java多线程的优势，采用JNA接口调用c++核心计算库，通过传入数据调用c++核心库完成计算。</p>

<p><strong>应用分析：</strong></p>

<p>1、一开始我们研究了一下JNI，JNA接口，从性能角度考虑我们选择了JNA的接口，因为批量计算模块处理的数据量非常的大，我们希望在细节上面都去把控他的性能；</p>

<p>2、在数据传递方面因为账本、账单都是一条条记录方式，正常情况都是以String、struct或者容器一类的对象进行传递，但是在经过大量的改造测试，发现就基于struct的数据传递跑通，但是经常会报出不同的内存错误问题，让人很头疼！</p>

<p>3、最后决定数据传递方面还是选择byte字节流方式最好，因此就有了本文一些小的做法。</p>

<p>4、账本和账单对于一个账户来讲会是多条，那么传递数据时候，就需要将这些多条进行拼接，拼接好后按照byte流在java和c++接口之间进行传递；</p>

<p>5、通过分析，拼接字符串方面StringBuffer是个比较高效的做法，相比java里面的String类型，因此java里面的数据采用StringBuffer存放；</p>

<p>6、通常String类型的字符串向byte的字节流转变提供了方便的接口getBytes()，但是StringBuffer到byte数组的转变得考虑怎么转换？</p>

<p>代码案例：</p>

<pre><code class="language-css">
/**
 * Created by wangfeng on 2014-11-18.
 */
public class testByte {
    public static void main(String argc[]){
        //---------get a StringBuffer
        StringBuffer buf=new StringBuffer();


        for(int i=0;i&lt;10;i++)
        {
            buf.append("i="+i+"\n");
        }
        System.out.println(buf);
        System.out.println(buf.length());
        //---------StringBuffer convert to byte
        byte[] retBuf=new byte[buf.length()];
        for(int j=0;j&lt;buf.length();j++)
        {
            retBuf[j]=Byte.parseByte((int)buf.charAt(j)+"");
        }
        //print byte
        String tmp=new String(retBuf);
        System.out.println(tmp);
    }
}
</code></pre>

<p>运行结果：</p>

<pre><code class="language-css">
i=0
i=1
i=2
i=3
i=4
i=5
i=6
i=7
i=8
i=9
40
i=0
i=1
i=2
i=3
i=4
i=5
i=6
i=7
i=8
i=9
</code></pre></div></body>
