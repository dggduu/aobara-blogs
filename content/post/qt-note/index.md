+++
title = 'Qt Note'
date = "2025-02-17"
categories = [
    "笔记",
]
+++
## 创建一个组件/窗口/线程类
对于xxx.h
```cpp
    #include <QWidget>
    or #include <Qwindow>
    or #include <QThread>
    or #include <QOpenGLWidget>
    class XXX : public QWidget   //继承于组件/窗口/线程类
    {
        Q_OBJECT
        public:
            explicit XXX(QWidget* parent = nullptr);
            ~XXX() override;
        public slots:
        protected:
        signals:
        private slots:
        private:
    };
```
对于XXX.cpp
```cpp
#include <XXX.h>
XXX::XXX(QWidget* parent)
: QWidget(parent)
,...//初始化成员数组
{
    ...
}

XXX::~XXX()
{
	delete ...;
}
```

## 添加部件
``` c
#include <QMenu>
#include <qaction.h>
#include <Qmessagebox>
...

QMenu * menu =new Menu();//记得销毁指针

	trayIcon = new QSystemTrayIcon(this);
	trayIcon->setToolTip("live2dViewer");
	trayIconMenu = new ElaMenu(this);
	trayIconMenu->setFixedWidth(150);
	QAction * HitokotoEnableSwitch = trayIconMenu->addElaIconAction(ElaIconType::Message,"启用一言");
	HitokotoEnableSwitch->setCheckable(true);
	QAction* settingAction = trayIconMenu->addElaIconAction(ElaIconType::Gear,"设置");
	QAction* quitAction = trayIconMenu->addElaIconAction(ElaIconType::Xmark,"退出");
```
## 属性
```c
    //window 属性
	this->setWindowFlag(Qt::FramelessWindowHint);
	this->setWindowFlag(Qt::WindowStaysOnTopHint);
	this->setWindowFlag(Qt::Tool);
	this->setAttribute(Qt::WA_TranslucentBackground);
	this->setFixedSize(500,500);
    
    //plainTextEdit属性
    petDialogBox->setGeometry(x,y,w, h);//设置大小
    petDialogBox->setFixedWidth(200);
    petDialogBox->setHorizontalScrollBarPolicy(Qt::ScrollBarAlwaysOff);
    petDialogBox->setVerticalScrollBarPolicy(Qt::ScrollBarAlwaysOn);
    petDialogBox->ensureCursorVisible();
    petDialogBox->setWordWrapMode(QTextOption::WrapAtWordBoundaryOrAnywhere);
```
## 多线程
```cpp
#inlcude <QThread>
    class MyThread : public QObject{
        signals:
            void finish();//emit MyThread::finish();生成信号
    }

    MyThread * thread = new MyThread();
    thread.start();

    ~ParentWidget(){
        thread.quit();
        thread.wait();//记得销毁
    }
```
## QTime
### QTimer 中断器
```cpp
#include <QTimer>
	timer = new QTimer(this);
	connect(timer,&QTimer::timeout,this, [=]() {
		update();
		});
	timer->start(1000/60);
```
## 鼠标事件 键盘事件
```cpp
#include <QKeyEvent>
#include <QMouseEvent>

void QtGLCore::keyReleaseEvent(QKeyEvent *event){//重写事件

}
void QtGLCore::mouseMoveEvent(QMouseEvent* event){
    if (this->isLeftButton)
    LAppDelegate::GetInstance()->GetView()->OnTouchesMoved(event->position().x(), event->position().y());
}

if (event->key() == Qt::Key_Alt)
```
## Network
```cpp
#include <QtNetwork/QNetworkAccessManager>
#include <QtNetwork/QNetworkReply>>
#include <QUrl>
QNetworkAccessManager *manager;


QUrl url("http://127.0.0.1:45327/");
QNetworkRequest request(url);
request.setHeader(QNetworkRequest::ContentTypeHeader, "application/json");

//get方法
    QUrl url("http://127.0.0.1:8000");
    QNetworkRequest request(url);
    manager->get(request);
```
## Qt JSON 处理
### 解析
```cpp
#include <QJsonObject>
#include <QJsonArray>
#include <QJsonDocument>
QByteArray response = reply->readAll();
        QJsonDocument doc = QJsonDocument::fromJson(response);

        QString rawContent = doc.object()["choices"].toArray().at(0).toObject()["message"].toObject()["content"].toString();

```
### 创建
```cpp
QJsonObject messageObject;
    messageObject["model"] = "repository@q6_k";//创建键值对

    QJsonArray messages;//创建数组
    messages.append(QJsonObject{{"role", "system"}, {"content", "Always answer in rhymes."}});//插入元素
    messages.append(QJsonObject{{"role", "user"}, {"content", message}});

    messageObject["messages"] = messages;

    manager->post(request, QJsonDocument(messageObject).toJson());
```
## 正则表达式
```cpp
 QRegularExpression thinkBlockRegex("<think>.*?</think>\\n\\n",
                     QRegularExpression::DotMatchesEverythingOption);
```
## QSS（就是CSS）
```cpp
        button.setStyleSheet(
        "QPushButton {"
        "background-color: #4CAF50;"
        "color: white;"
        "border: none;"
        "padding: 10px 20px;"
        "text-align: center;"
        "font-size: 16px;"
        "}"
    );
```
## 属性动画
```cpp
#include <qpropertyanimation.h>
void QtGLCore::showEvent(QShowEvent * event) {					//缓入动画（1s）
	QPropertyAnimation *animetion = new QPropertyAnimation(this, "windowOpacity");
	animetion->setDuration(1000);
	animetion->setStartValue(0.0);
	animetion->setEndValue(1.0);
	animetion->start(QPropertyAnimation::DeleteWhenStopped);
}
void QtGLCore::fadeOutAnimation() {
	QPropertyAnimation *fadeAnimation = new QPropertyAnimation(this, "windowOpacity");
	fadeAnimation->setDuration(1000);
	fadeAnimation->setStartValue(1.0);  // 从完全不透明开始
	fadeAnimation->setEndValue(0.0);    // 到完全透明结束
	fadeAnimation->start(QPropertyAnimation::DeleteWhenStopped);
	connect(fadeAnimation, &QPropertyAnimation::finished, [=]() {
		this->setVisible(false);
	});
}
void QtGLCore::quitApplication() {								//缓出动画（500ms）
	animation = new QPropertyAnimation(this, "windowOpacity");
	animation->setDuration(500);
	animation->setStartValue(0.0);
	animation->setEndValue(1.0);
	connect(animation, &QPropertyAnimation::finished, [=]() {
		animation->deleteLater();
		QApplication::quit();
		});
	animation->start(QPropertyAnimation::DeleteWhenStopped);
}
```