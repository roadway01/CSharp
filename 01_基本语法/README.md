# C#系列教程 - 基础语法篇


## 一、partial class
在WinForm用地很多，每个界面基本都有`业务代码文件`如`Form1.cs`和`界面配置管理文件`如`Form1.Designer.cs`中，并且二者都利用`partial class`实现了同一个类如`Form1`

前者放用户真正关系的业务代码，后者放自动生成的界面代码、资源配置代码、系统变量等不需要用户关系的东西，很好地实现了业务和UI的分离

举例如下：

#### 1.业务逻辑文件：Form1.cs
> ` public partial class Form1 : Form`里面实现了业务逻辑，一般进入这个文件的方式如下：

+ 1.双击窗体，进入Form1的初始化函数中
+ 2.双击控件，进入Form1中控件对应的响应函数中
+ 3.想直接自己写方法或者类，也可以直接从项目文件目录中点击进入，直接加代码

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

namespace WindowsFormStudy
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {

        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }
    }
}

```

#### 2.界面配置管理文件：Form1.Designer.cs
> 这里的类型和上面的Form1.cs里的是相同的，都是`partial class Form1`，但是这个里面的代码都是自动生成地，在项目初始化、添加页面控件、修改控件属性等的时候都会自动刷新这个文件，我们平时不用关系

```csharp
namespace WindowsFormStudy
{
    partial class Form1
    {
        /// <summary>
        /// 必需的设计器变量。
        /// </summary>
        private System.ComponentModel.IContainer components = null;

        /// <summary>
        /// 清理所有正在使用的资源。
        /// </summary>
        /// <param name="disposing">如果应释放托管资源，为 true；否则为 false。</param>
        protected override void Dispose(bool disposing)
        {
            if (disposing && (components != null))
            {
                components.Dispose();
            }
            base.Dispose(disposing);
        }

        #region Windows 窗体设计器生成的代码

        /// <summary>
        /// 设计器支持所需的方法 - 不要
        /// 使用代码编辑器修改此方法的内容。
        /// </summary>
        private void InitializeComponent()
        {
            this.button1 = new System.Windows.Forms.Button();
            this.SuspendLayout();
            // 
            // button1
            // 
            this.button1.Location = new System.Drawing.Point(123, 97);
            this.button1.Name = "button1";
            this.button1.Size = new System.Drawing.Size(75, 23);
            this.button1.TabIndex = 0;
            this.button1.Text = "button1";
            this.button1.UseVisualStyleBackColor = true;
            this.button1.Click += new System.EventHandler(this.button1_Click);
            // 
            // Form1
            // 
            this.AutoScaleDimensions = new System.Drawing.SizeF(6F, 12F);
            this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
            this.ClientSize = new System.Drawing.Size(370, 341);
            this.Controls.Add(this.button1);
            this.Name = "Form1";
            this.Text = "Form1";
            this.Load += new System.EventHandler(this.Form1_Load);
            this.ResumeLayout(false);

        }

        #endregion

        private System.Windows.Forms.Button button1;
    }
}
```

## 二、getter和setter

### 1.类似Java的写法
```csharp
class Student
{
    private string name;
    // Getter
    public string GetName()
    {
        return this.name;
    }
    // Setter
    public void SetName(string name)
    {
        this.name = name;
    }
}
```

### 2.初步简化
```csharp
class Student
{
    private string name;
    public String Name
    {
        get
        {
            return this.name;
        }
        set
        {
            this.name = value;
        }
    }
}
```

### 3.使用lambda表达式来简化
```csharp
class Student
{
    private string name;

    public string Name
    {
        get => name;
        set => name = value;
    }
}
```

### 4.最简化形式
> 注意此时的属性要直接大写了

```csharp
class Student
{
    public string Name { get; set; }
}
```

## 三、重写override
如果子类想对父类的方法实现进行修改，可以重写该方法。在重写时，要求
+ 父类将方法声明为 `virtual`（虚拟方法，表示可能会被重写的）。
+ 子类将方法声明为 `override`（表示重写）。

例子如下：

父类：
```csharp
class Example
{
    // 。。。略
    public virtual void Translate()
    {
        Console.WriteLine("number:" + number);
    }
}
```

子类如下：
```csharp
class MyExample : Example
{
    public override void Translate()
    {
        Console.WriteLine("号码为:" + number);
    }
}
```

> 注意：想要在在子类中调用父类的方法，java中用`super.xxx()`，C#中用`base.xxx()`，上面的例子中在MyExample中想要调用Example中的Translate()应该用`base.Translate()`


> 想要在子类中使用父类的构造函数，情况特殊点，和Java不同，和C++类似。举例如下：


定义父类 Point。示例代码如下。

```csharp
public class Point
{
    public int x, y;
    public Point(int x, int y)
    {
        this.x = x;
        this.y = y;
    }
}
```
定义子类 Point3D,示例代码如下。
```csharp
public class Point3D : Point
{
    public int z;
    public Point3D(int x, int y, int z) : base(x, y)
    {
        this.z = z;
    }
}
```
其中，在子类的构造方法之后，显式地调用了 base(x, y) ，表示对父类构造方法的显式调用。

## 四、接口interface
使用 interface 关键字可以定义一个接口。

例如，定义一个接口 IAudipPlayer，表示音频播放功能。注意， 接口的名字应

该以大写的 I 字母开头。
```csharp
// 定义一个接口，接口名要以 I 开头
public interface IAudioPlayer
{
    void Play(int[] data);
}
```

### 五、List的使用
```csharp
List<int> list = new List<int>();
list.Add(12);
list.Add(3);
list.Add(8);
list.Add(19);
Console.WriteLine(list[2]); // 可以直接用索引来访问
list[3] = 666; // 直接用索引来更新
for (int i = 0; i < list.Count(); i++) // fori
{
    Console.WriteLine(list[i] + " ");
}
foreach (int item in list) // foreach
{
    Console.WriteLine(item + " ");
}
```