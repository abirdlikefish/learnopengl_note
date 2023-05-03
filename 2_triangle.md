

### 名词

**Normalized Device coordinates (NDC)**
标准化设备坐标
[-1,1]^3

**Vertex Shader**
顶点着色器

**Primitive assembly**
图元装配

**Geometry Shader**
几何着色器

**Rasterization**
光栅化

**Fragment Shader**
片段着色器

**Blending**
混合

**Vertex Buffer Objects (VBO)**
顶点缓冲对象
在显存中储存大量顶点

**Vertex Array Object (VAO)**
顶点数组对象

**Element Buffer Object (EBO)**
元素缓冲对象
**Index Buffer Object (IBO)**
索引缓冲对象


### glGenBuffer
```cpp

//generate buffer object names
//n 生成的缓冲的数量  
//buffers 存储缓冲ID的数组地址
void glGenBuffers(GLsizei n , GLuint * buffers);

```

### glBindBuffer
```cpp
//bind a named buffer object
//target 缓冲对象类型
//buffer 缓冲ID
void glBindBuffer(GLenum target,GLuint buffer);

```
+ GL_ARRAY_BUFFER
+ GL_ELEMENT_ARRAY_BUFFER


### glBufferData
```cpp
float vertices[] = {
    -0.5f, -0.5f, 0.0f,
     0.5f, -0.5f, 0.0f,
     0.0f,  0.5f, 0.0f
};

//将vertices里的数据写到缓存中
glBufferData(GL_ARRAY_BUFFER , sizeof(vertices) , vertices , GL_STATIC_DRAW );
```
+ GL_ARRAY_BUFFER
+ GL_ELEMENT_ARRAY_BUFFER


+ GL_STATIC_DRAW    数据基本不变
+ GL_DYNAMIC_DRAW   数据多次改变
+ GL_STREAM_DRAW    数据每次绘制都会改变


### glGenVertexArrays
```cpp

glGenVertexArrays( 1, &VAO );
```

### glBindVertexArray



### glVertexAttribPointer
```c
// 设置顶点属性
// index 顶点属性位置值
// size 一个顶点属性的大小
// type 顶点属性的类型
// normalizes 是否要标准化
// stride 步长 , 相邻的顶点属性间的间隔
// pointer 偏移量
void glVertexAttribPointer(GLuint index, GLint size, GLenum type, GLboolean normalized, GLsizei stride, const GLvoid* pointer);

glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);

```

### glEnableVertexAttribArray
```c
// 启用顶点
```


### 链接顶点属性

```c
//配置顶点属性
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0 );

// 启用位置值为0的顶点属性
glEnableVertexAttribArray(0);

```

### VBO

```cpp
unsigned int VBO;   //缓冲ID
glGenBuffers(1,&VBO);//生成一个VBO对象,获得其缓冲ID
glBindBuffer(GL_ARRAY_BUFFER,VBO);//把VBO绑定到GL_ARRAY_BUFFER上
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```

### EBO
```c
unsigned int EBO;
glGenBuffers(1, &EBO);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
```

### VAO

```c
// 创建并绑定一个顶点数组对象
unsigned int VAO;
glGenVertexArrays( 1, &VAO );
glBindVertexArray(VAO);

// 绑定VBO和EBO
glBindBuffer(GL_ARRAY_BUFFER, VBO);	// and a different VBO
glBufferData(GL_ARRAY_BUFFER, sizeof(vertex), vertex, GL_STATIC_DRAW);

glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);

// 绑定顶点属性
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);

```



### glCreateShader
```cpp

GLuint glCreateShader（GLenum shaderType）;
返回创建的shader的ID

//创建着色器
unsigned int shader = glCreateShader(/*shader type like*/ GL_VERTEX_SHADER);

```

+ GL_VERTEX_SHADER
+ GL_FRAGMENT_SHADER


### glShaderSource
```c

//shader shader的ID
//count 源码的字符串的串数
//string 源码
//length 指定字符串长度的数组
void  glShaderSource（GLuint shader，GLsizei count，const GLchar * const *string，const GLint *length）;

glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);

```

### glCompileShader
```c
//编译
void glCompileShader（GLuint shader）;

```

### glCreateProgram

```c
//创建着色程序
unsigned int shaderProgram;
shaderProgram = glCreateProgram();

```


### glAttachShader
``glAttachShader( shaderProgram, vertexShader );``


### glLinkProgram
``glLinkProgram(shaderProgram);``

### glUseProgram
``glUseProgram(shaderProgram);``

### glDeleteShader
``glDeleteShader(vertexShader);``

### glDeleteBuffers
``    glDeleteBuffers(1,&VBO);``

### glDeleteProgram
``    glDeleteProgram(shaderProgram);``

### glDeleteVertexArrays
``    glDeleteVertexArrays(1,&VAO);``

### Vertex Shader

##### source code
```c
//版本声明为3.3版本,使用核心模式
#version 330 core   

//设定输入的顶点属性位置值为 0 
//in 关键词 声明 输入顶点属性为 vec3 , 变量名为 aPos
layout (location = 0 ) in vec3 aPos;    

void main()
{
    gl_Position = vec4(aPos.x , aPos.y , aPos.z , 1.0 );    //设置输出
}

```

##### compile

``` c
//储存源码
const char *vertexShaderSource = "#version 330 core\n"
    "layout (location = 0) in vec3 aPos;\n"
    "void main()\n"
    "{\n"
    "   gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);\n"
    "}\0";

```

```c
//创建shader
unsigned int vertexShader;
vertexShader = glCreateShader(GL_VERTEX_SHADER);
```

```c
//将源码附到shader上
glShaderSource(vertexShader,1,&vertexShaderSource,NULL);
```

```c
//编译
glCompileShader(vertexShader);
```

```c
//检测编译状态

int  success;
char infoLog[512];  //错误信息
glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success);

if(!success)
{
    glGetShaderInfoLog(vertexShader, 512, NULL, infoLog);
    std::cout << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n" << infoLog << std::endl;
}

```

### Fragment Shader

##### source code
```c
//设置版本号与模式
#version 330 core

//声明输出变量
out vec4 FragColor;

void main()
{
    FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);
} 

```

##### compile

```c

unsigned int fragmentShader;
fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);
glCompileShader(fragmentShader);

```

### Shader Program

```c
//创建着色程序
unsigned int shaderProgram;
shaderProgram = glCreateProgram();

```

```c
//将两个顶点附在shaderProgram上
glAttachShader( shaderProgram, vertexShader );
glAttachShader( shaderProgram, fragmentShader );

//链接
glLinkProgram(shaderProgram);

```

```c
//检测链接是否成功
glGetProgramiv(shaderProgram, GL_LINK_STATUS, &success);
if(!success) {
    glGetProgramInfoLog(shaderProgram, 512, NULL, infoLog);
    ...
}
```


```c
//激活该程序对象
glUseProgram(shaderProgram);

```

```c
//删除已经链接了的着色器对象
glDeleteShader(vertexShader);
glDeleteShader(fragmentShader);
```


### glDrawArrays
```c
// 绘制图形
// mode 绘制模式
// first 从第first个顶点开始绘制
// count 绘制顶点数
void glDrawArrays(  GLenum mode,    GLint first,    GLsizei count);  

```

+ GL_POINTS：把每一个顶点作为一个点进行处理，顶点n即定义了点n，共绘制N个点

+ GL_LINES：连接每两个顶点作为一个独立的线段，顶点2n和2n+1之间共定义了n条线段，总共绘制N/2条线段

+ GL_LINE_STRIP：绘制从第一个顶点到最后一个顶点依次相连的一组线段，第n和n+1个顶点定义了线段n，总共绘制n－1条线段

+ GL_LINE_LOOP：绘制从第一个顶点到最后一个顶点依次相连的一组线段，然后最后一个顶点和第一个顶点相连，第n和n+1个顶点定义了线段
n，总共绘制n条线段

+ GL_TRIANGLES：把每三个顶点作为一个独立的三角形，顶点3n、3n+1和3n+2定义了第n个三角形，总共绘制N/3个三角形

+ GL_TRIANGLE_STRIP：绘制一组相连的三角形,对于偶数n，顶点n、n+1和n+2定义了第n+ 个三角形，对于奇数n，顶点n、n+2和n+1定义了第n个三角形;总共绘制N-2个三角形

+ GL_TRIANGLE_FAN：绘制一组相连的三角形，三角形是由第一个顶点及其后给定的顶点确定，顶点0、n+1和n+2定义了第n个三角形，总共绘制N-2个三角形


### glDrawElements
```c
// 按索引绘制图形
// mode 绘制模式
// count 绘制顶点数
// type 索引类型
// 索引指针
glDrawElements( GLenum  mode , GLsizei count , GLenum  type , const GLvoid  *indices);

glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);

```


