#include <iostream>
#include <string>
using namespace std;
// 加密功能
string myEncrypt(string text, string key) {
    string newtext;  // 用来装结果 
    int keynum = key.size(); // 钥匙长度   
    // 逐个处理每个字母
    for(int i=0; i<text.size(); i++){
        char nowChar = text[i];  // 当前要处理的字母 
        // 钥匙不够长就循环用
        int keypos = i;
        while(keypos >= keynum){
            keypos = keypos - keynum; // 手动计算循环位置 
        }
        char nowKey = key[keypos];  
        // 小写字母a-z对于的ASCII码为97-122，故减去97即可一一对应，如a=97，b=98，z=122等，所以a对应移动0位，b对应移动1位，以此类推 
        int num1 = nowChar - 97;    // 字母转数字（a=0）
        int num2 = nowKey - 97;     // 钥匙转数字 
        int num3 = num1 + num2;     // 直接相加 
        // 如果超过z就从头开始
        if(num3 >=26){
            num3 = num3 -26;
        }
        newtext += char(num3 +97);  // 转回字母 
    }
    return newtext;
}
int main(){
    string inputText, myKey;   
    // 输入提示（带重复）
    cout << "输入要加密的文字（小写）：";
    cin >> inputText;
    cout << "输入你的秘密钥匙（小写）：";
    cin >> myKey;    
    // 直接调用函数 
    cout << "加密结果是：" << myEncrypt(inputText, myKey);
}
