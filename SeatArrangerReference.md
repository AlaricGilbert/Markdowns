# SeatArranger库开发思路~~与正确使用姿势~~
~~我才不会说这个库是答应的某班长的委托的日常跳票操作~~

## 库源码文件说明
|类名|SeatArranger|Peoples|PeopleCombination|RelationshipsManager|  
|:-:|:-:|:-:|:-:|:-:|
|对应文件名|SeatArranger.cs|Peoples.cs|PeopleCombination.cs|RelationshipsManager.cs|  
|公有|√|×|×|×|
|可序列化|×|对|√|√|
|作用|外部调用封装|根据编号获取人的姓名|用于存储人的固定组合（关系）的类|对人与人之间的关系进行增减更变存储读取|

## 开发思路及源码说明
### Peoples.cs
就是一个类似枚举类型的类没啥可看的→_→
~~暴露了同学们的姓名应该不会打我吧~~
```cs
namespace Alaric.SeatArranger
{
    class Peoples {
        static string[] People ={"彭  奥","杨新宇","朱玥澄","邱  丹","石雯昕","汤舒仪",
        "胡冰晴","许皓钦","桂雨涵","康静怡","刘国庆","刘子龙","姚  丰","郭熙宇","黄远昊",
        "尹希至","蒋月明","鲁  力","张美威","尹浩安","陈洪伟","张  衡","魏鸣之","杨  凯",
        "高子淇","鲍嫣然","周欣欣","龙  婕","汪  冲","蒋子龙","林崎帆","沈子皓","夏  铁",
        "陈佳敏","李佳辰","韩  煜","翟羽佳","程  卓","程小倩","胡佳奇","张紫千","田思举",
        "肖晨薇","李飞杨","刘晨光","魏宇帆","肖世寰","聂纹莎","陈慕华","刘  念","孙佳彬",
        "卢建益","毛永昊","张  孟","廖心怡","黄雨泽","黄毅成","艾子天","朱龙康","饶华涛",
        "徐李阳","陆鹤灵","刘雅伦","李沐阳","王  依","舒婉怡"};
        //根据编号返回其对应人的姓名
        public static string Get(int i)
        {
            return People[i];
        }
    }
}
```
### PeopleCombination.cs
```cs
using System;
using System.Collections;

namespace SeatArranger
{
    [Serializable]
    public class PeopleCombination
    {
        //存储人物间关系的ArrayList
        private ArrayList bond = new ArrayList();
        //构造函数 可用int型可变参数初始化数据
        //如:new PeopleCombination(11,23) 返回的是11号与23号组成的PeopleCombination
        //   new PeopleCombination(11,23,24) 返回的是11号、23号、24号按其顺序组成的PeopleCombination 
        public PeopleCombination(params int[] data) => bond.AddRange(data);
        //获取内部人物关系数据
        public ArrayList GetInnerData() => bond;
        //添加数据组合
        public virtual void AddRange(ArrayList list) => bond.AddRange(list);
        public virtual void AddRange(PeopleCombination pc) => AddRange(pc.bond);
        public static PeopleCombination Combine(PeopleCombination pc1, PeopleCombination pc2)
        {
            pc1.AddRange(pc2);
            return pc1;
        }
        //组合中人物数
        public int Count() => bond.Count;
        //获取该组合中指定人物的序号
        public int GetIndexOf(int A) => bond.IndexOf(A);
        //反向排列
        public void Reverse() => bond.Reverse();
        //判断该组合中是否包含指定人物
        public bool Contians(int A) => bond.Contains(A);
        //添加数据
        public void Add(int A) => bond.Add(A);
        //分割人物组合
        public PeopleCombination[] Split(int firstCombinationCount)
        {
            PeopleCombination[] result = new PeopleCombination[2];
            result[0] = new PeopleCombination();
            result[1] = new PeopleCombination();
            for (int i = 0; i < bond.Count; i++)
            {
                if (i < firstCombinationCount)
                    result[0].Add((int)bond[i]);
                else
                    result[1].Add((int)bond[i]);
            }
            return result;
        }
        //转为字符串
        public override string ToString()
        {
            string r = "";
            for (int i = 0; i < bond.Count; i++)
            {
                r += Peoples.Get((int)bond[i]);
                r += "  ";
            }
            return r.Substring(0, r.Length - 2);
        }
        //在控制台中打印
        public void Print() => Console.WriteLine(this.ToString());
    }
}
```