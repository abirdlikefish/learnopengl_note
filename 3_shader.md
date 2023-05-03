### 数据类型
+ vecn	包含n个float分量的默认向量
+ bvecn	包含n个bool分量的向量
+ ivecn	包含n个int分量的向量
+ uvecn	包含n个unsigned int分量的向量
+ dvecn	包含n个double分量的向量

```c
vec2 someVec;
vec4 differentVec = someVec.xyxx;
vec3 anotherVec = differentVec.zyw;
vec4 otherVec = someVec.xxxx + anotherVec.yxzy;
```

```c
vec2 vect = vec2(0.5, 0.7);
vec4 result = vec4(vect, 0.0, 0.0);
vec4 otherResult = vec4(result.xyz, 1.0);
```

### glfwGetTime()
```c
    // 获得时间
    // 秒为单位,有小数
```

### glGetUniformLocation
```c
// 查询已激活的program里的uniform 的 ID
GLint glGetUniformLocation（GLuint programID,const GLchar *uniformName）;

```

### glUniform
```c
// 设置uniform
glUniform4f(unifromID, 0.0f, 0.0f, 0.0f, 1.0f);
```
+ f	函数需要一个float作为它的值
+ i	函数需要一个int作为它的值
+ ui	函数需要一个unsigned int作为它的值
+ 3f	函数需要3个float作为它的值
+ fv	函数需要一个float向量/数组作为它的值

### 文件输入输出

```c
Shader(const char* vertexPath, const char* fragmentPath)
{
    // 1. 从文件路径中获取顶点/片段着色器
    std::string vertexCode;
    std::string fragmentCode;
    std::ifstream vShaderFile;
    std::ifstream fShaderFile;
    // 保证ifstream对象可以抛出异常：
    vShaderFile.exceptions (std::ifstream::failbit | std::ifstream::badbit);
    fShaderFile.exceptions (std::ifstream::failbit | std::ifstream::badbit);
    try 
    {
        // 打开文件
        vShaderFile.open(vertexPath);
        fShaderFile.open(fragmentPath);
        std::stringstream vShaderStream, fShaderStream;
        // 读取文件的缓冲内容到数据流中
        vShaderStream << vShaderFile.rdbuf();
        fShaderStream << fShaderFile.rdbuf();       
        // 关闭文件处理器
        vShaderFile.close();
        fShaderFile.close();
        // 转换数据流到string
        vertexCode   = vShaderStream.str();
        fragmentCode = fShaderStream.str();     
    }
    catch(std::ifstream::failure e)
    {
        std::cout << "ERROR::SHADER::FILE_NOT_SUCCESFULLY_READ" << std::endl;
    }
    const char* vShaderCode = vertexCode.c_str();
    const char* fShaderCode = fragmentCode.c_str();
    [...]
}
```

