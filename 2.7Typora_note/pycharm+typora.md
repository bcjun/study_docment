# Pycharm远程开发

### 配置远程地址映射

![pycharm远程开发](D:%5C%E5%B8%B8%E7%94%A8%E8%B5%84%E6%96%99%5C%E7%AC%94%E8%AE%B0%5C2.7Typora_note%5C%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87%5Cpycharm%E8%BF%9C%E7%A8%8B%E5%BC%80%E5%8F%91.png)





![pycharm远程开发1](D:%5C%E5%B8%B8%E7%94%A8%E8%B5%84%E6%96%99%5C%E7%AC%94%E8%AE%B0%5C2.7Typora_note%5C%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87%5Cpycharm%E8%BF%9C%E7%A8%8B%E5%BC%80%E5%8F%911.png)





![pycharm远程开发2](D:%5C%E5%B8%B8%E7%94%A8%E8%B5%84%E6%96%99%5C%E7%AC%94%E8%AE%B0%5C2.7Typora_note%5C%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87%5Cpycharm%E8%BF%9C%E7%A8%8B%E5%BC%80%E5%8F%912.png)



### 配置远程环境

![pycharm远程开发3](D:%5C%E5%B8%B8%E7%94%A8%E8%B5%84%E6%96%99%5C%E7%AC%94%E8%AE%B0%5C2.7Typora_note%5C%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87%5Cpycharm%E8%BF%9C%E7%A8%8B%E5%BC%80%E5%8F%913.png)



![pycharm远程开发4](D:%5C%E5%B8%B8%E7%94%A8%E8%B5%84%E6%96%99%5C%E7%AC%94%E8%AE%B0%5C2.7Typora_note%5C%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87%5Cpycharm%E8%BF%9C%E7%A8%8B%E5%BC%80%E5%8F%914.png)



### 自动设置远程提交

![pycharm远程自动提交](D:%5C%E5%B8%B8%E7%94%A8%E8%B5%84%E6%96%99%5C%E7%AC%94%E8%AE%B0%5C2.7Typora_note%5C%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87%5Cpycharm%E8%BF%9C%E7%A8%8B%E8%87%AA%E5%8A%A8%E6%8F%90%E4%BA%A4.png)

# Pycharm中运行Flask

![Flaks1.1版本在Pycharm中执行](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/Flaks1.1%E7%89%88%E6%9C%AC%E5%9C%A8Pycharm%E4%B8%AD%E6%89%A7%E8%A1%8C.png)

# Typora

**表情符号** 😄

使用语法输入表情符号`:smile:`。



输入`[toc]`并`Return`按键

### mermaid 语法

> **流程图**
>
> ~~~mermaid
> graph TD;
>     A-->B;
>     A-->C;
>     B-->D;
>     C-->D;
> ~~~
>
> **PS:**
>
> ~~~
> TB/TD - top bottom
> BT - bottom top
> RL - right left
> LR - left right
> ~~~
>
> ~~~mermaid
> graph TB
>     Start --> Stop
> ~~~
>
> **节点**
>
> ~~~mermaid
> graph LR
>     id
> ~~~
>
> **带文字**
>
> ~~~mermaid
> graph LR
>     id[带文字节点]
> ~~~
>
> ~~~mermaid
> graph LR
>     id((圆形节点))
> ~~~
>
> ```mermaid
> graph LR
>     id(圆角节点)
> ```
>
> ```mermaid
> graph LR
>     id>不对称节点]
> ```
>
> ```mermaid
> graph LR
>     id{菱形节点}
> ```

##### 连接线

> ~~~mermaid
> graph LR
>     A-->B
> ~~~
>
> ```mermaid
> graph LR
>     A---B
> ```
>
> ```mermaid
> graph LR
>     A-- 文字 ---B
> ```
>
> ```mermaid
> graph LR
>     A-- 文字 -->B
> ```
>
> ```mermaid
> graph LR;
>    A-.->B;
> ```
>
> ```mermaid
> graph LR
>    A-. text .-> B
> ```
>
> ```mermaid
> graph LR
>    A ==> B
> ```
>
> ```mermaid
> graph LR
>    A == text ==> B
> ```

##### 子图

> ~~~mermaid
> graph TB
>     c1-->a2
>     subgraph one
>     a1-->a2
>     end
>     subgraph two
>     b1-->b2
>     end
>     subgraph three
>     c1-->c2
>     end
> ~~~

##### 样式

> ~~~mermaid
> graph TD;
>     A-->B;
>     A-->C;
>     B-->D;
>     C-->D;
>     linkStyle 0 stroke:#0ff,stroke-width:2px;
>     linkStyle 3 stroke:#ff3,stroke-width:4px;
> ~~~
>
> ```mermaid
> graph LR
>     id1(Start)-->id2(Stop)
>     style id1 fill:#f9f,stroke:#333,stroke-width:4px
>     style id2 fill:#ccf,stroke:#f66,stroke-width:2px,stroke-dasharray: 5, 5
> ```

##### 时序图

> ~~~mermaid
> sequenceDiagram
>     participant Alice
>     participant Bob
>     Alice->John: Hello John, how are you?
>     loop Healthcheck
>         John->John: Fight against hypochondria
>     end
>     Note right of John: Rational thoughts <br/>prevail...
>     John-->Alice: Great!
>     John->Bob: How about you?
>     Bob-->John: Jolly good!
> ~~~
>
> ~~~mermaid
> sequenceDiagram
>     Alice->>John: Hello John, how are you?
>     John-->>Alice: Great!
> ~~~
>
> ```mermaid
> sequenceDiagram
>     participant John
>     participant Alice
>     Alice->>John: Hello John, how are you?
>     John-->>Alice: Great!
> ```
>
> ```mermaid
> sequenceDiagram
>     A->B: 无箭头实线
>     A-->B: 无箭头虚线(点线)
>     A->>B: 有箭头实线
>     A-->>B: 有箭头实线
>     A-x B: 有箭头实线，加上叉
>     A--x B: 有箭头虚线，加上叉
> ```

​	