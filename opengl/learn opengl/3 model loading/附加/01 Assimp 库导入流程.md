
https://assimp.sourceforge.net/lib_html/index.html

```mermaid
graph TB
	A[Scene] --> B1[Material]
	A --> B2[mesh]
	B1 --> C1[texture]
	B2 --> C2[Face , Vertex , Normal , UV , ... ]
```

其中face 相当于 index

官方代码
```cpp
#include <assimp/Importer.hpp>      // C++ importer interface
#include <assimp/scene.h>           // Output data structure
#include <assimp/postprocess.h>     // Post processing flags
#include "print.h"
bool import(const std::string& pFile)
{
    Assimp::Importer importer;
    const aiScene* scene = importer.ReadFile(pFile,
        aiProcess_CalcTangentSpace |
        aiProcess_Triangulate |
        aiProcess_JoinIdenticalVertices |
        aiProcess_SortByPType);

    if (!scene)
    {
        print(importer.GetErrorString());
        return false;
    }
    print("导入成功");
    return true;
}


int main()
{
    import("./resources/IronMan/IronMan.obj");
}
```


