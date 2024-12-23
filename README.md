
在`Streamlit`中，布局类组件扮演着至关重要的角色。


它们不仅决定了应用程序的视觉呈现和用户体验，也极大地增强了页面内容的组织性和可读性。


通过这些组件，开发者可以灵活地划分页面空间，创建出清晰、有条理的布局结构。


本篇主要介绍**3种**构建`Streamlit App`时常用的3种布局类组件：


* `st.container`：用于封装和组合多个组件，形成统一的视觉单元
* `st.columns`：将内容以并列的方式展示，提高信息展示的效率和效果
* `st.expander`：提供了可折叠的面板功能，使得额外信息可以在需要时展开查看，既节省了空间又保持了界面的整洁性


# 1\. st.container


`st.container`通过将多个组件放入一个容器中，可以轻松地控制这些组件的布局和样式。


`st.container`本身并不直接提供布局参数，一般是通过`with`语句将要包含的组件放入容器中，


或者，通过嵌套使用其他布局组件（如`st.column`和`st.row`等）来在容器内部实现更复杂的布局。


## 1\.1\. 使用示例


假定这样一个场景，在一个数据分析应用中，需要同时展示数据表格和相关的文字说明。



```
import streamlit as st
import pandas as pd

# 创建示例数据
data = pd.DataFrame(
    {
        "A": [1, 2, 3],
        "B": [4, 5, 6],
        "C": [7, 8, 9],
    }
)

# 使用st.container封装数据表格和说明文字
with st.container():
    st.dataframe(data)
    st.markdown("这是数据表格的说明，提供了对数据的简要描述和分析。")

```

![](https://img2024.cnblogs.com/blog/83005/202411/83005-20241123150112831-1207944110.png)


或者在一个机器学习模型演示应用中，需要同时展示模型预测结果和交互式控件（如滑块），


也可以通过`st.container`，将预测结果和滑块控件组合在一起，形成一个交互式的界面



```
# 创建一个滑块控件
slider_value = st.slider("调整滑块以查看不同结果", 0, 100)

# 使用st.container封装预测结果和滑块控件
with st.container():
    st.write(f"当前滑块值为：{slider_value}")
    st.write("这是一个简单的机器学习模型演示。通过调整滑块，你可以看到不同的预测结果。")

```

![](https://img2024.cnblogs.com/blog/83005/202411/83005-20241123150112804-748464873.gif)


# 2\. st.columns


`st.columns`组件用于创建一个列布局的容器，它可以将页面内容分割成多个垂直排列的列。


当需要在页面上同时展示多个并列的组件或信息块时，可以考虑使用`st.columns`。


`st.columns`参数很简单，它使用一个整数作为参数，该整数指定要创建的列数。


## 2\.1\. 使用示例


首先，模拟一个数据可视化应用中，需要并列展示一个折线图和相关的文字说明的场景。



```
import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

# 创建示例数据
data = pd.DataFrame(
    {
        "X": [1, 2, 3, 4, 5],
        "Y": [10, 15, 13, 17, 16],
    }
)

# 绘制折线图
plt.plot(data["X"], data["Y"])
plt.xlabel("X-Axis")
plt.ylabel("Y-Axis")
plt.title("chart example")

# 使用st.columns并列展示图表和文字说明
col1, col2 = st.columns(2)
col1.pyplot(plt)
col2.write("这是一个折线图示例，展示了X轴和Y轴之间的关系。")

```

![](https://img2024.cnblogs.com/blog/83005/202411/83005-20241123150113187-1082736946.png)


再模拟一个数据对比的应用，需要同时展示多个数据表格以便进行比较和分析。



```
# 创建示例数据
data1 = pd.DataFrame({"A": [1, 2, 3], "B": [4, 5, 6]})

data2 = pd.DataFrame({"A": [7, 8, 9], "B": [10, 11, 12]})

# 使用st.columns创建三列布局展示数据表格
col1, col2, col3 = st.columns(3)
col1.dataframe(data1)
col2.dataframe(data2)
col3.write("这是两个数据表格的对比展示。通过并列展示，你可以更方便地进行比较和分析。")

```

![](https://img2024.cnblogs.com/blog/83005/202411/83005-20241123150113433-1310412090.png)


# 3\. st.expander


`st.expander`组件用于创建一个可折叠的面板，允许用户点击以展开或隐藏面板内的内容。


当需要在页面上提供额外的信息或选项，但又不希望这些信息始终可见时，可以考虑使用`st.expander`。


`st.expander`将一个字符串作为参数，该字符串将作为面板的标题显示。


然后，可以在`with`语句块内添加要在面板中显示的组件。


## 3\.1\. 使用示例


首先模拟一个数据分析的应用，其中包含一个详细设置的面板，但默认情况下这些设置是隐藏的。



```
import streamlit as st

# 创建一个包含详细设置的st.expander面板
with st.expander("详细设置"):
    st.write("这里是一些详细的设置选项，如数据过滤、排序等。")
    st.slider("数据过滤阈值", 0, 100)
    st.checkbox("启用排序功能")

# 主界面内容模拟
st.write("这是一个数据分析应用的主界面。你可以点击上面的“详细设置”来查看和修改设置。")

```

![](https://img2024.cnblogs.com/blog/83005/202411/83005-20241123150113447-1977495394.gif)


此外，在一个比较复杂的Web应用中，如果需要提供一个包含帮助文档和指南的面板，以便用户在使用时参考。


那么，可以使用`st.expander`，将帮助文档和指南隐藏在一个可折叠的面板中，


用户可以在需要时查看这些信息，而不会影响主界面的展示效果。



```
with st.expander("帮助文档和指南"):
    st.markdown(
        """
    # 帮助文档和指南

    欢迎使用本应用！这里是一些常用的操作和技巧，帮助你更好地使用本应用。

    - **如何开始**：点击左侧导航栏中的“开始”按钮，进入应用主界面。
    - **数据导入**：在主界面上方点击“导入数据”按钮，上传你的数据文件。
    - **数据分析**：在数据导入后，点击“分析数据”按钮，选择你想要进行的分析类型。

    如有任何疑问，请联系技术支持。
    """
    )

# 主界面模拟
st.write("这是一个复杂的Web应用。如果你需要帮助或指南，请点击上面的“帮助文档和指南”。")

```

![](https://img2024.cnblogs.com/blog/83005/202411/83005-20241123150113171-1121467216.gif)


# 4\. 总结


这**3个组件**在布局时的侧重点不同，其中，`st.container`侧重组件封装，`st.columns`侧重列布局，而`st.expander`侧重信息隐藏。


我们在使用时根据应用的具体展现形式来选择，


比如，需要组织复杂布局时则用`st.container`，需要并列展示信息时用`st.columns`，需要隐藏额外信息时用`st.expander`。


 本博客参考[豆荚加速器](https://yirou.org)。转载请注明出处！
