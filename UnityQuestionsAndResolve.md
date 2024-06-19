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

