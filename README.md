![téléchargement](https://user-images.githubusercontent.com/96391221/146688592-d36925a3-35d8-4e78-875a-d4968a4e32e6.png)  
![abdellh](https://user-images.githubusercontent.com/96391221/150834237-7fdc3bee-675d-4557-a18a-84cf404da882.png)

## <span style="color:red">Abstract:</span>

* * *
The goal of the homework is to create an application to manage you tasks. It should have all the features of main application such as menues, actions and toolbar. The application must store an archive of all the pending and finished tasks.

* * *
* * *

#### main.cpp
```cpp
#include "tache.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    Tache w;
    w.show();
    return a.exec();
}


```
