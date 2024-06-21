# Unity开发中遇到的一些问题及解决

## 1.改变子对象在UI层次结构(Hierarchy)中的顺序

转载自：[如何改变子对象在UI层次结构中的顺序 - CSDN文库](https://wenku.csdn.net/answer/4c0wpyivt3)

```
using UnityEngine;
using UnityEngine.UI;

public class ChangeOrder : MonoBehaviour
{
    public Transform parentTransform; // Vertical Layout Group的父对象
    public Transform childTransform; // 需要改变位置的子对象

void Start()
{
    // 将子对象移动到目标位置
    childTransform.SetSiblingIndex(0);
    // 更新Vertical Layout Group的子对象顺序
    LayoutRebuilder.ForceRebuildLayoutImmediate(parentTransform.GetComponent<RectTransform>());
}

}
```

在Start()方法中，我们首先使用SetSiblingIndex()将子对象移动到目标位置，这会改变U层次结构中子对象的顺序。然后，我们使用LayoutRebuilder.ForceRebuildLayoutImmediate()强制更新Vertical Layout Group的子对象顺序。

**具体应用：**

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class GenerateButton : MonoBehaviour
{
    public Transform parentTrans;
    public Button btnSelf;
    // Start is called before the first frame update
    void Start()
    {
        btnSelf.onClick.AddListener(() =>
        {
            GlobalData.index++;
            GameObject obj = Instantiate(Resources.Load<GameObject>("btn"),FindObjectOfType<VerticalLayoutGroup>().transform);
            obj.name = GlobalData.index.ToString();
            obj.GetComponentInChildren<Text>().text = name.Replace("btn","");
            obj.transform.SetSiblingIndex(transform.GetSiblingIndex() + 1);
            GlobalData.Instance.generateObjs.Add(obj);
            LayoutRebuilder.ForceRebuildLayoutImmediate(parentTrans.GetComponent<RectTransform>());
        });
    }
}
```

## 2.解决Unity UGUI滑动组件自动回弹的问题

在我们使用Unity自带的滑动组件（Scroll View）时，有时候在Content下填充了物体之后，滑动会出现回弹的情况，也就是划不到最下边，无法看到最下边的元素，如下图所示

![img](D:\Unity资料和笔记\个人笔记\UnityQuestionsAndResolve-Image\20210302200111574.gif)

原因就在于 填充的物体所占的体积大于了Content的大小。如图所示，红色为Content的大小，而黄色为需要滑动的物体所占的大小。

![img](D:\Unity资料和笔记\个人笔记\UnityQuestionsAndResolve-Image\20210302200436248.png)

解决方案：可以在Content上添加Content Size Fitter和Layout Group组件，这样可以让Content去根据子元素自适应大小，从而保证正常滑动。

![image-20240620093736125](D:\Unity资料和笔记\个人笔记\UnityQuestionsAndResolve-Image\image-20240620093736125.png)

                            版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

原文链接：https://blog.csdn.net/wang568270833/article/details/114293129
