# CALance
简单的计算器APP

主要思路：

利用数据结构的四则运算，中缀转后缀，后缀再计算结果；

本计算器是利用集合List中的ArrayList来存储的；

主要分三部分：

1.将字符串分割出来；

2.将中缀表达式转化成后缀表达式；

3.将后缀表达式计算结果；

代码如下：

1.将字符串分割出来：

public class jiequ_str {

    public String jiequ_str(char[] b){
        int count_zuo = 0,count_you = 0;
        String t;
        List<String> c = new ArrayList<String>();       //利用集合List中的ArrayList来存储字符数组;
        String a = "";

        for (int i = 0; i < b.length; i++) {
            if(b[i] == '+'){
                if(!a.equals("")){
                    c.add(a);
                    a = "";
                }
                c.add(b[i]+"");

            }else if(b[i] == '-'){
                if(i==0||b[i-1] == '('){
                    a += b[i];
                }else{
                    if(!a.equals("")){
                        c.add(a);
                        a = "";
                    }
                    c.add(b[i]+"");
                }
            }else if(b[i] == '*'){
                if(!a.equals("")){
                    c.add(a);
                    a = "";
                }
                c.add(b[i]+"");
            }else if(b[i] == '/'){
                if(!a.equals("")){
                    c.add(a);
                    a = "";
                }
                c.add(b[i]+"");
            }else if(b[i] == '='){
                if(!a.equals("")){
                    c.add(a);
                    a = "";
                }
                c.add(b[i]+"");
            }else if(b[i] == '(')
            {
                if(count_you>count_zuo){    //判断，如果右括号比左括号先出现的话，则输出"ERROR";
                    return "ERROR";
                }
                count_zuo++;        //累计左括号的数量；
                if(!a.equals("")){
                    c.add(a);
                    a = "";
                }
                c.add(b[i]+"");
            }else if(b[i] == ')'){
                count_you++;        //累计右括号的数量；
                if(!a.equals("")){
                    c.add(a);
                    a = "";
                }
                c.add(b[i]+"");
            }else if(b[i]=='出'){    //如果字符数组的第一个字符等于"出"，则输出"出错"；
                return "出错";
            }
            else {
                if ((b[i] == '.' && i > 0 && i < b.length-1 && (b[i+1]=='.'))||( i== 0&&b[i] == '.')
                        ||(b[i] == '.' && i > 0 && i < b.length-3 && (b[i+1]=='.'||b [i+2]=='.')) ){
                    return "ERROR";  //如果小数点连续出现或者间接出现的话，则输出"ERROR"；
                }
                a += b[i];
            }
        }
        if(count_you != count_zuo)      //如果左括号与右括号的数量不一致的话，则输出"ERROR";
        return "ERROR";

        for(int i=0;i<c.size();i++)     //在编译器上显示;
            System.out.println(c.get(i));

       t = new mid_to_end().mid_to_end(c);
        return t;
    }
}

2.将中缀表达式转化成后缀表达式：

public class mid_to_end {

    public String mid_to_end(List<String> c){
        String t;
        int x=-1,y=-1;
        List<String> a = new ArrayList<String>();	//用List中的ArrayList来存符号;
        List<String> b = new ArrayList<String>();	//用List中的ArrayList来存数字;
        System.out.println("size:"+c.size());      //在编译器上显示;
        for(int i=0;i<c.size();i++){
            if(c.get(i).equals("(")){
                x++;
                a.add('('+"");
            }else if(c.get(i).equals(")")){
                if(a.get(x).equals("(")){
                    a.remove(x);
                    x--;
                }else{
                    while(!a.get(x).equals("(")){
                        y++;
                        b.add(a.get(x));
                        a.remove(x);
                        x--;
                    }
                    a.remove(x);
                    x--;
                }
            }else if(c.get(i).equals("+")||c.get(i).equals("-")){

                if(x!=-1&&(a.get(x).equals("+")||a.get(x).equals("-")
                        ||a.get(x).equals("*")||a.get(x).equals("/"))){
                    y++;
                    b.add(a.get(x));
                    a.remove(x);
                    a.add(c.get(i));
                }else{
                    x++;
                    a.add(c.get(i));
                }
            }else if(c.get(i).equals("*")||c.get(i).equals("/")){

                if(x!=-1&&(a.get(x).equals("*")||a.get(x).equals("/"))){
                    y++;
                    b.add(a.get(x));
                    a.remove(x);
                    a.add(c.get(i));
                }else{
                    x++;
                    a.add(c.get(i));
                }
            }else if(c.get(i).equals("=")){
                while(x!=-1){
                    y++;
                    b.add(a.get(x));
                    x--;
                }
            }else{
                y++;
                b.add(c.get(i));
            }

        }
       t = new end_to_result().end_to_result(b);
        return t;
    }
}

3.将后缀表达式计算结果：

public class end_to_result {

    public String end_to_result(List<String> b){

        List<String> x = new ArrayList<String>(); //创建一个新的ArrayList；

        int y = b.size();
        int z = y;
        double a;

        while(y!=0){            //将b中的元素倒入新的ArrayList x中;
            x.add(b.get(y-1));
            y--;
        }
        //开始进行将元素进行计算;
        for(int i=z-1;i>=0;i--) {
            if (x.get(i).equals("+")) {
                if(i==x.size()-1||i==x.size()-2){
                    return "ERROR";
                }
                a = Double.valueOf(x.get(i + 2)) + Double.valueOf(x.get(i + 1));
                x.remove(i + 2);
                x.remove(i + 1);
                x.set(i,a + "");
            } else if (x.get(i).equals("-")) {
                if(i==x.size()-1||i==x.size()-2){
                    return "ERROR";
                }
                a = Double.valueOf(x.get(i + 2)) - Double.valueOf(x.get(i + 1));
                x.remove(i + 2);
                x.remove(i + 1);
                x.set(i, a + "");
            } else if (x.get(i).equals("*")) {
                if(i==x.size()-1||i==x.size()-2){
                    return "ERROR";
                }
                a = Double.valueOf(x.get(i + 2)) * Double.valueOf(x.get(i + 1));
                x.remove(i + 2);
                x.remove(i + 1);
                x.set(i, a + "");
            } else if (x.get(i).equals("/")) {
                if(i==x.size()-1||i==x.size()-2){
                    return "ERROR";
                }
                a = Double.valueOf(x.get(i + 2)) / Double.valueOf(x.get(i + 1));
                x.remove(i + 2);
                x.remove(i + 1);
                x.set(i,a + "");
            } else {
                continue;
            }
        }
        if(x.size() == 0)
            return 0+"";        //如果文本内容的长度为0，则输出0;
        return Double.valueOf(x.get(0))+"";
    }
}
