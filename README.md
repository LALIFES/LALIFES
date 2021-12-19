![téléchargement](https://user-images.githubusercontent.com/96391221/146688592-d36925a3-35d8-4e78-875a-d4968a4e32e6.png)

## main.cpp
```cpp
#include "mainwindow.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();
    return a.exec();
}





```


## mainwindow.cpp
```cpp


#include "mainwindow.h"
#include "ui_mainwindow.h"
#include"QFileDialog"
#include"QMessageBox"
#include"QTextStream"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    setCentralWidget(ui->plainTextEdit);
    currentfile=nullptr;
    setWindowTitle("buffer");
}

MainWindow::~MainWindow()
{
    delete ui;
}


void MainWindow::on_actionnew_triggered()
{
   ui->plainTextEdit->setPlainText(QString());
}


void MainWindow::on_actionopen_triggered()
{
    auto filename= QFileDialog::getOpenFileName();
        QFile file(filename);
        currentfile=new QString(filename);
        if(!file.open(QIODevice::ReadOnly | QFile::Text)){
            QMessageBox::warning(this,"warning","cannot open file:"+file.errorString());
              return;
        }
        setWindowTitle(filename);
        QTextStream in(&file);
        QString text=in.readAll();
        ui->plainTextEdit->setPlainText(text);
        file.close();
}


void MainWindow::on_actionsave_triggered()
{
    if(!currentfile){
        auto filename= QFileDialog::getSaveFileName();
        QFile file(filename);
        currentfile=new QString(filename);
        if(!file.open(QFile::WriteOnly | QFile::Text)){
            QMessageBox::warning(this,"warning","cannot save file:"+file.errorString());
            return;
        }
        setWindowTitle(filename);
        QTextStream out(&file);
        QString text=ui->plainTextEdit->toPlainText();
        out <<text;
        file.close();
        }
}


void MainWindow::on_actionsave_as_triggered()
{
    if(currentfile){
        auto filename= QFileDialog::getSaveFileName();
        QFile file(filename);
        currentfile=new QString(filename);
        if(!file.open(QFile::WriteOnly | QFile::Text)){
            QMessageBox::warning(this,"warning","cannot save file:"+file.errorString());
            return;
        }
        setWindowTitle(filename);
        QTextStream out(&file);
        QString text=ui->plainTextEdit->toPlainText();
        out <<text;
        file.close();
        }
}


void MainWindow::on_actionexit_triggered()
{
    QApplication::quit();
}


void MainWindow::on_actioncut_triggered()
{
    ui->plainTextEdit->cut();
}


void MainWindow::on_actioncopy_triggered()
{
   ui->plainTextEdit->copy();
}


void MainWindow::on_actionpaste_triggered()
{
     ui->plainTextEdit->paste();
}




```
## mainwindow.h
```h
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>

QT_BEGIN_NAMESPACE
namespace Ui { class MainWindow; }
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private slots:
    void on_actionnew_triggered();

    void on_actionopen_triggered();

    void on_actionsave_triggered();

    void on_actionsave_as_triggered();

    void on_actionexit_triggered();

    void on_actioncut_triggered();

    void on_actioncopy_triggered();

    void on_actionpaste_triggered();

private:
    Ui::MainWindow *ui;
    QString *currentfile;
};
#endif // MAINWINDOW_H

```
## mainwindow.ui

![Capture d’écran 2021-12-19 210606](https://user-images.githubusercontent.com/96391221/146689316-ba54a9a4-ad59-4cf1-8e8e-030dda5e4938.png)

* * *
Enfin on a la resultat suivantes


![Capture d’écran 2021-12-19 211727](https://user-images.githubusercontent.com/96391221/146689571-a8dd747c-2d83-42b1-b78e-5a0e32c75642.png)

