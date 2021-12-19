![téléchargement](https://user-images.githubusercontent.com/96391221/146688592-d36925a3-35d8-4e78-875a-d4968a4e32e6.png)


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
