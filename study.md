###  HelloBolt6

 #### 自定义控件

> 该例子演示了如何自定义一个控件，自定义的是一个Button ，除了Bolt 定义的一些元控件，其他控件基本都需要进行自定义

+ 类型自定义：自定义控件可以自定义的类型 control ，这个在nametable.cfg 里面设置关联的时候和 type 属性对应；

+ 类名自定义： `class="HelloBolt.button"`  , nametable.cfg 里面定义的时候class 属性对应，并且在调用的界面中 class 也需要是这里定义的名字 ` <obj id="userdefine.btn" class="HelloBolt.button">`

+ 自定义属性：可以对控件进行属性的定义，设置属性的值的类型和默认值

  ```xml
  <attr_def>
      <attr name="HoverBkgID" type="string" >
          <default>button.hover</default>
      </attr>
      <attr name="Text" type="string" />
  </attr_def>   
  ```

  + 属性值的获取, 通过调用自定义控件实例的GetAttribute() 方法

    ```lua
    local attr = self:GetAttribute()  -- 获取控件的attr 所有属性
    self:SetText(attr.Text)			-- 获取其中的 Text 属性并调用SetTex方法，该方法是对界面的text 控件内容进行赋值
    ```

    

+ 自定义对外提供的方法：

  ```xml
  <method_def>
  	<SetState file="Button.lua" func="SetState"/>
  	<SetText file="Button.lua" func="SetText" />
  </method_def>
  ```

  + 首先自定义方法需要在对应的lua 文件中进行实现；
  + 这样定义的方法可以作为自定义控件的成员函数进行使用；

+ 自定义响应事件：

  ```xml
  <event_def>
  	<!-- 定义一个自定义事件，这样按钮的使用者就可以连这个逻辑事件-->
  	<OnClick />
  </event_def>
  ```

  + 这样在使用者中可以使用 event name="OnClick" 的事件

    ```xml
    <eventlist>
        <event name="OnClick" file="MainWnd.xml.lua" func="userdefine_btn_OnClick"/>
    </eventlist>
    ```

    

+ 自定义控件的UI组成：

  + objtreetemplate 的节点便是xml 里面进行UI自定义的部分 
  + 这里根对象定义的是id="bkg" 的TextureObject,然后在其子元素中定义id="oldbkg" 的 TextureObject 和 一个 TextObject ，用于显示按钮的背景和文案，在OnBind 的时候进行文案的显示的，OnBind是元素绑定到Obj树上的时候触发执行。
  + 鼠标移动时候按钮还实现了一个动画效果，实现了状态的动态变化；

