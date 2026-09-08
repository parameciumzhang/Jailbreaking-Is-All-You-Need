# Jailbreaking Is All You Need

## 项目介绍
目的: 构建一个**多场景、多模型通用**的越狱框架

由于需要覆盖尽可能地场景，因此没有对特定的模型做针对性的越狱逻辑优化(后续会不断更新，针对特定系列的模型和针对特定场景的优化)。但是，该越狱框架在效果和多场景层面已经做到了**平衡**，能够在不同模型和不同场景上实现一致的越狱效果。

初衷还是通过**one-shot**(用户实际对话层面的one-shot)直接达到越狱效果。尽管该越狱框架支持多轮对话，但是如果第一次未越狱成功，还是建议手动退出(q)重新运行。

## 使用方法
```
在.env文件中配置API_Key、BASE_URL、Model后
直接双击运行jailbreaking_is_all_you_need.exe即可
```
## 效果演示
开始界面:
![效果演示](./static/1.png)
输入危险问题后越狱成功展示: 本人代码能力有限，工具的性能和交互展示效果并不是最优的。输出的越狱结果建议手动unicode解码查看。
![效果演示](./static/3.png)
由于模型生成随机性的问题，需要多次尝试one-shot越狱。且不同模型的越狱效果可能会有差异(详见实验结果表)。
![效果演示](./static/2.png)


## 实验结果

由于资金有限和本人专业能力有限，在尽可能覆盖多个主流模型的基础上，每个模型进行了15次越狱实验。

故成功越狱的概率仅供参考。

该表均为one-shot单次越狱实验结果。

| langchain integrations | model             | temperature               | reasoning\_effort         | total | success | fales | jailbreaking rate |
| :--------------------- | :---------------- | :------------------------ | :------------------------ | :---- | :------ | :---- | :---------------- |
| ChatOpenAI             | kimi-3            | None(follow model defult) | None(follow model defult) | 15    | 4       | 11    | 27%               |
| ChatOpenAI             | kimi-2.7-code     | None(follow model defult) | None(follow model defult) | 15    | 10      | 5     | 66%               |
| ChatOpenAI             | deepseek-v4-pro   | None(follow model defult) | None(follow model defult) | 15    | 8       | 7     | 53%               |
| ChatOpenAI             | deepseek-v4-flash | None(follow model defult) | None(follow model defult) | 15    | 12      | 3     | 80%               |
| ChatOpenAI             | glm-5.3           | None(follow model defult) | None(follow model defult) | 15    | 0       | 15    | 0%                |
| ChatOpenAI             | glm-5.3-flash     | None(follow model defult) | None(follow model defult) | 15    | 7       | 8     | 47%               |
| ChatOpenAI             | qwen3.8-max       | None(follow model defult) | None(follow model defult) | 15    | 2       | 13    | 14%               |
| ChatOpenAI             | qwen3.8-flash     | None(follow model defult) | None(follow model defult) | 15    | 0       | 15    | 0%                |
| ChatOpenAI             | qwen3.8-27b       | None(follow model defult) | None(follow model defult) | 15    | 2       | 14    | 14%               |

说明：

- `None(follow model defult)` 表示跟随模型默认值，比如`kimi-2.7-code`和`kimi-3`模型的temperature默认值为固定为`1.0`，`kimi-3`模型的`reasoning_effort`默认值为`max`；`qwen3.8-max`（仅思考模式）`temperature`默认值为`0.6`；`deepseek-v4-pro`（仅思考模式）`temperature`默认值为`1.0`,`reasoning_effort`默认值为`high`...


## 个人瞎唠
本人非人工智能科班，只能从安全角度去理解模型越狱。网上看过很多师傅的越狱文章和论文，各种方法角色扮演、长上下文稀释、信任偏差、编码变体等等，但个人觉得大部分方法都是**引导和干扰模型的注意力**，将模型的注意力聚焦在不该聚焦的地方。这些方法本质没什么区别。

模型内部机制是个黑盒，再加上本质就是概率生成，故目前没有完全统一的越狱方法论。
提示词的顺序、数量、甚至是单一字符都会影响越狱的效果。
所以针对单一场景和单一模型针对性的构造越狱提示词是效果最好的，但当下新模型不断发布，安全对齐的方法也在不断改进，需要去不断地更新越狱逻辑。

这种不断循环的场景并不少见，像SQL注入不断地添加黑名单字典来防护、大模型不断地增大上下文来扩展记忆。但这些都会存在阈值天花板，效果会逐渐减弱。
跳出这种循环的解决方法无一例外都是使用了新的模式而不是在原来的逻辑上不断更新:SQL注入采用预编译，大模型通过harness管理上下文压缩或按需加载等等。

所以不停的改变提示词逻辑是无法真正跳出这种循环的，我们更需要去寻找一个全新的越狱模式。