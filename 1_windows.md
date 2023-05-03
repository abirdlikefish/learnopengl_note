
### 初始化
``` cpp
glfwInit();     //初始化GLFW
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);//设置主版本号
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);//设置次版本号
glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);//使用核心模式

```


### 创建窗口
```cpp
GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);//设置窗口大小,名称

//判断是否创建成功
if (window == NULL)
{
    std::cout << "Failed to create GLFW window" << std::endl;
    glfwTerminate();
    return -1;
}

glfwMakeContextCurrent(window);//设置为主上下文
```


### 初始化GLAD
```cpp
if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
{
    std::cout << "Failed to initialize GLAD" << std::endl;
    return -1;
}
```

### 视口
```cpp
glViewport(0, 0, 800, 600);  //设置视口左下角坐标,长,宽

```

### 回调函数

```cpp
void framebuffer_size_callback(GLFWwindow* window, int width, int height);//函数声明

//函数实现
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    glViewport(0, 0, width, height);
}

glfwSetFramebufferSizeCallback(window, framebuffer_size_callback); //注册该函数


```


### 渲染循环

```cpp
while(!glfwWindowShouldClose(window))//判断窗口是否被关闭
{
    processInput(window);//输入

    glClearColor(0.2f, 0.3f, 0.3f, 1.0f);//设置清屏颜色
    glClear(GL_COLOR_BUFFER_BIT);   //清屏

    //渲染
    //...

    glfwSwapBuffers(window);    //交换颜色缓冲
    glfwPollEvents();    //检查有无事件,调用回调函数
}


```


### 结束

```cpp
glfwTerminate();    //清理资源
return 0;

```


### 输入

```cpp

//按ESC退出
void processInput(GLFWwindow *window)
{
    if(glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)//未按下则为 GLFW_RELEASE
        glfwSetWindowShouldClose(window, true); //将 WindowShouldClose 属性设置为 true 
}

```


### 清屏

```cpp

glClearColor(0.2f, 0.3f, 0.3f, 1.0f);//设置清屏颜色
glClear(GL_COLOR_BUFFER_BIT);   //清屏

```


###

```cpp
```


###

```cpp
```

