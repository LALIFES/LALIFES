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
#### tache.cpp

```cpp
#include "tache.h"
#include "ui_tache.h"
#include "remtache.h"
#include <QSplitter>
#include <QMenu>
#include <QListView>
#include <QListWidget>
#include <QHBoxLayout>
#include <QFileSystemModel>
#include <QAbstractItemModel>
#include <QStringListModel>
#include <QToolBar>
#include <QApplication>
#include <QHBoxLayout>
#include <QVBoxLayout>
#include <QMenuBar>
#include <QPixmap>
#include <QMessageBox>
#include <QDate>
#include <QFile>
#include <QStandardItemModel>
#include <QTextStream>

Tache::Tache(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::Tache)
{
    ui->setupUi(this);
    today =new QStandardItemModel();
    done =new QStandardItemModel();
    futur =new QStandardItemModel();



    createActions();

    createMenus();

    createToolBars();

    makeConnexions();

    //declaration du fichier
    QFile fichierTaches("C:/Users/Bureau/Documents/Tasks/lesTaches.txt");
    if (!fichierTaches.open(QIODevice::ReadOnly | QIODevice::Text))
            return;

        while (!fichierTaches.atEnd()) {
            QString line = fichierTaches.readLine();
            QStandardItem *t = new QStandardItem();


            if(line.at(0)== "1"){

                sizeTaskToday.append(line);
                t->setIcon(QIcon(":/icons/new_file.png"));
                t->setText(line+" ");

                today->appendRow(t);

                ui->sizeTaskToday->setModel(today);
            }else if(line.at(0)== "2"){

                sizeTaskFutur.append(line);
                t->setIcon(QIcon(":/icons/pending_icon.png"));
                t->setText(line+" ");

                futur->appendRow(t);

                ui->sizeTaskFutur->setModel(futur);
            }else if(line.at(0)== "3"){
                sizeTaskDone.append(line);


                t->setIcon(QIcon(":/icons/finished_task_icon.png"));
                t->setText(line+" ");

                done->appendRow(t);

                ui->sizeTaskDone->setModel(done);
            }
        }


}

Tache::~Tache()
{
    delete ui;
    delete quit;
    delete newFile;
    delete saveFile;
    delete recent;

    delete finished;
    delete pending;

    delete file;
    delete option;
    delete helpe;



}


void Tache::createActions(){

    //-->  creer l'action new file
    QPixmap newIcon(":/icons/new_file.png");
    newFile=new QAction(newIcon,"&New File", this);
    newFile->setShortcut(tr("Ctrl+N"));
    //connect(newFile,&QAction::triggered, this, &QMainWindow::create);


    //-->  creer l'action de finished task
    QPixmap finishedTaskIcon(":/icons/finished_task_icon.png");
    finished=new QAction(finishedTaskIcon," &Finished Task", this);
    finished->setShortcut(tr("Ctrl+F"));


    //-->  creer l'action de pending task
    QPixmap pendingTaskIcon(":/icons/pending_task_icon.png");
    pending=new QAction(pendingTaskIcon," &Pending Task", this);
    pending->setShortcut(tr("Ctrl+P"));


    //-->  creer l'action de save
    QPixmap saveIcon(":/icons/save_file.png");
    saveFile=new QAction(saveIcon," &Save File", this);
    saveFile->setShortcut(tr("Ctrl+S"));


    //-->  creer l'action de quit
    QPixmap quitIcon(":/icons/quit_icon.png");
    quit = new QAction(quitIcon, "&Exit", this);
    quit->setShortcut(tr("Ctrl+Q"));
    //connect(quit, &QAction::triggered, this, &QMainWindow::close);



}

void Tache::createMenus(){
    //definir le menu de notre application
    file=menuBar()->addMenu("&File");
    file->addAction(newFile);
    file->addAction(saveFile);
    file->addSeparator();
    file->addAction(finished);
    file->addAction(pending);
    file->addSeparator();
    file->addAction(quit);

    menuBar()->addSeparator();

    option = new QMenu("&Options");
    menuBar()->addMenu(option);

    menuBar()->addSeparator();

    helpe = new QMenu("&Help");
    menuBar()->addMenu(helpe);
}

void Tache::createToolBars(){

    //creer un toolbar
    auto toolbar1 =addToolBar("&File1");

    //ajouter des actions a ce toolbar
    toolbar1->addAction(newFile);
    toolbar1->addSeparator();
    toolbar1->addAction(finished);
    toolbar1->addSeparator();
    toolbar1->addAction(pending);
    toolbar1->addSeparator();
    toolbar1->addAction(quit);

}

void Tache::makeConnexions(){

    //connetion for new file
    connect(newFile,&QAction::triggered, this, &Tache::newTache);

    //connection for the exit button
    connect(quit, &QAction::triggered, this, &Tache::close);

}

void Tache::newTache(){
    remTache R;

    auto D= R.exec();
    if(D==remTache::Accepted){
            QString text = R.getDescription();
            QStandardItem *t = new QStandardItem;


            if(R.getDate()==QDate::currentDate() && !R.isChecked()){
                t->setIcon(QIcon(":/icons/task_icon.png"));
                t->setText("1 "+text+" ");

                today->appendRow(t);

                ui->sizeTaskToday->setModel(today);
                sizeTaskToday.append(text);

            }
            else if(R.getDate()!=QDate::currentDate() && !R.isChecked()){

                t->setIcon(QIcon(":/icons/pending_task_icon.png"));
                t->setText("2 "+text+" ");

                futur->appendRow(t);

                ui->sizeTaskFutur->setModel(futur);
                sizeTaskFutur.append(text);

            }
            else if(R.isChecked()){
                t->setIcon(QIcon(":/icons/finished_task_icon.png"));
                t->setText("3 "+text+" ");

                done->appendRow(t);

                ui->sizeTaskDone->setModel(done);
                sizeTaskDone.append(text);

            }
        }
}
void Tache::closeEvent(QCloseEvent *e)
{
    QFile fichierTaches("C:/Users/PC/Documents/DataBase/lesTaches.txt");
        if (fichierTaches.open(QIODevice::ReadWrite| QIODevice::Text)){

            QTextStream out(&fichierTaches);
            for (int i=0;i<sizeTaskToday.size() ;i++ ) {
                out<< "1"<< sizeTaskToday[i] << Qt::endl;
            }
            for (int i=0;i<sizeTaskFutur.size() ;i++ ) {
                out<< "2"<< sizeTaskFutur[i] << Qt::endl;
            }
            for (int i=0;i<sizeTaskDone.size() ;i++ ) {
                out<< "3"<< sizeTaskDone[i] << Qt::endl;
            }
            fichierTaches.close();
        }
}

```
####remtache.cpp
```cpp
#include "remtache.h"
#include "ui_remtache.h"
#include <QDate>
#include <QDateTimeEdit>
#include <QString>

remTache::remTache(QWidget *parent) :
    QDialog(parent),
    ui(new Ui::remTache)
{
    ui->setupUi(this);
    updateDate();
}

remTache::~remTache()
{
    delete ui;
}

void remTache::updateDate(){
    QDate D=QDate::currentDate();
    ui->dateEdit->setMinimumDate(D);
    ui->dateEdit->setDate(D);

}

QString remTache::getDescription() const{
    return ui->lineEdit->text() +"Due : "+ ui->dateEdit->text() + " Tag : "+ ui->comboBox->currentText() + " ";
}

QDate remTache::getDate() const{
    return ui->dateEdit->date();
}

bool remTache::isChecked() const{
    if(ui->checkBox->isChecked())
        return true;
    return false;
}

```
