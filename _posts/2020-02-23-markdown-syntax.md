---
layout: post
title: Markdown 语法——从入门到美化
tags: [教程, Markdown]
author: Todd Zhou
show_author_profile: true
pageview: false
key: 2020-02-23-markdown-syntax
comment: true
mathjax: false
header:
  theme: dark
  background: '#0'
article_header:
  type: cover
  align: center
  image:
    src: https://cdn.makeuseof.com/wp-content/uploads/2014/05/WhatIsMarkdown_Lead_Image-994x400.jpg
---

Markdown 致力于使阅读和创作文档变得容易.
<!--more-->
<!-- more -->

# 为什么用 Markdown

&emsp;&emsp;Markdown 是一种轻量级**「标记语言」**，通过给文字加上标记，可以对文字进行排版。说是语言，其实 Markdown 的语法十分简单，你可以很快学会。



# 基本语法

下面展示的是 Markdown 最本质的 12 个语法：

<div markdown="0">
  <table markdown="0">
    <thead>
      <tr>
        <th>Type（兼容性好）</th>
        <th>Or（兼容性差）</th>
        <th>… to Get</th>
        <th>注释</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>*Italic*</td>
        <td>_Italic_</td>
        <td><em>Italic</em></td>
      </tr>
      <tr>
        <td>**Bold**</td>
        <td>__Bold__</td>
        <td><strong>Bold</strong></td>
      </tr>
      <tr>
        <td>
          # Heading 1
        </td>
        <td>
          Heading 1<br />
          =========
        </td>
        <td>
          <h1>Heading 1</h1>
        </td>
      </tr>
      <tr>
        <td>
          ## Heading 2
        </td>
        <td>
          Heading 2<br />
          ---------
        </td>
        <td>
          <h2>Heading 2</h2>
        </td>
        <td>
          最小的是6级，即6个#
        </td>
      </tr>
      <tr>
        <td>
          [Link](http://a.com)
        </td>
        <td>
          [Link][1]<br />
          ⋮<br />
          [1]: http://b.org
        </td>
        <td><a href="https://commonmark.org/">Link</a></td>
      </tr>
      <tr>
        <td>
          ![Image](http://url/a.png "注释")
        </td>
        <td>
          ![Image][1]<br />
          ⋮<br />
          [1]: http://url/b.jpg
        </td>
        <td>
          <img
            src="https://commonmark.org/help/images/favicon.png"
            width="36"
            height="36"
            alt="Markdown"
            title="注释"
          />
        </td>
        <td>
          把鼠标移到图片上会出现「注释」
        </td>
      </tr>
      <tr>
        <td>
          &gt; Blockquote
        </td>
        <td>
          &nbsp;
        </td>
        <td>
          <blockquote>Blockquote</blockquote>
        </td>
      </tr>
      <tr>
        <td>
          <p>
            * List<br />
            * List<br />
            * List
          </p>
        </td>
        <td>
          <p>
            - List<br />
            - List<br />
            - List<br />
          </p>
        </td>
        <td>
          <ul>
            <li>List</li>
            <li>List</li>
            <li>List</li>
          </ul>
        </td>
      </tr>
      <tr>
        <td>
          <p>
            1. One<br />
            2. Two<br />
            3. Three
          </p>
        </td>
        <td>
          <p>
            1) One<br />
            2) Two<br />
            3) Three
          </p>
        </td>
        <td>
          <ol>
            <li>One</li>
            <li>Two</li>
            <li>Three</li>
          </ol>
        </td>
      </tr>
      <tr>
        <td>
          Horizontal Rule<br />
          <br />
          ---
        </td>
        <td>
          Horizontal Rule<br />
          <br />
          ***
        </td>
        <td>
          Horizontal Rule
          <hr />
        </td>
        <td>
          这个是分割线
        </td>
      </tr>
      <tr>
        <td>
          `Inline code` with backticks
        </td>
        <td>
          &nbsp;
        </td>
        <td><code>Inline code</code> with backticks</td>
        <td><kbd>`</kbd>这个键在 <kbd>1</kbd> 左边</td>
      </tr>
      <tr>
        <td>
          ```<br />
          # code block<br />
          print '3 backticks or'<br />
          print 'indent 4 spaces'<br />
          ```
        </td>
        <td>
          <span>····</span># code block<br />
          <span>····</span>print '3 backticks or'<br />
          <span>····</span>print 'indent 4 spaces'
        </td>
        <td>
          <div>
            # code block
            <br />
            print '3 backticks or' <br />
            print 'indent 4 spaces'
          </div>
        </td>
        <td>
          在开头 <kbd>```</kbd> 后面加上语言，会自动高亮<br />
          （例子中没写）
        </td>
      </tr>
    </tbody>
  </table>
</div>



---




掌握了上面的语法，即可开始用 Markdown 写作。这里有一个非常好的 [在线教程](https://commonmark.org/help/tutorial/) 。

