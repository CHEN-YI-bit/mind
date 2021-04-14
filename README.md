# mind
mqtt
/*!
 * MindPlus
 * mpython
 *
 */
#include <MPython.h>
#include <DFRobot_Iot.h>
// 函数声明
void onButtonAPressed();
void obloqMqttEventT0(String& message);
// 静态常量
const String topics[5] = {"j1pOWUlMR","C80OWUlGR","","",""};
const MsgHandleCb msgHandles[5] = {obloqMqttEventT0,NULL,NULL,NULL,NULL};
// 创建对象
DFRobot_Iot myIot;


// 主程序开始
void setup() {
	mPython.begin();
	myIot.setMqttCallback(msgHandles);
	buttonA.setPressedCallback(onButtonAPressed);
	myIot.wifiConnect("602iot", "18wulian");
	while (!myIot.wifiStatus()) {yield();}
	display.setCursorLine(1);
	display.printLine("success");
	myIot.init("iot.dfrobot.com.cn","CBJBus_MR","","jf1Bus_GRz",topics,1883);
	myIot.connect();
	while (!myIot.connected()) {yield();}
	display.setCursorLine(2);
	display.printLine("MQTT连接成功");
}
void loop() {

}

// 事件回调函数
void onButtonAPressed() {
	display.fillScreen(0);
	myIot.publish(topic_0, "hello");
	display.fillScreen(0);
	display.setCursorLine(1);
	display.printLine("发送");
}
void obloqMqttEventT0(String& message) {
	display.fillScreen(0);
	display.setCursorLine(1);
	display.printLine(message);
	rgb.write(-1, 0x0000FF);
	delay(20000);
	rgb.write(-1, 0x000000);
	display.fillScreen(0);
}
