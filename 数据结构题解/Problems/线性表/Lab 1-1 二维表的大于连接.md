
总是出现运行时错误？

```cpp
Node* temp1=head1->next;

    while(temp1!=nullptr)

    {//遍历R的第i行（共遍历m1行）

        //在单行遍历中，找到该行col为x的结点，保存其data为t1；

        while (temp1&&temp1->col!=x)

        {

            temp1=temp1->next;

        }

        if(temp1){

            int t1=temp1->data;

        Node* temp2=head2->next;

        while(temp2!=nullptr)

        {//然后遍历S表的第j行（共m2行），在单行遍历中，找到该行col为y的结点，保存其val为t2；

            while (temp2&&temp2->col!=y)

            {

                temp2=temp2->next;

            }

            if(temp2){

                int t2=temp2->data;

            //如果t1>t2，那么连接当前R表的第i行和S表的第j行

            if(t1>t2){

                Node* tail1=head1->next;

                int num1=temp1->row;

                while(tail1&&tail1->row!=num1){

                    tail1=tail1->next;

                }

                while (tail1&&tail1->row==num1)

                {

                    outfile<<tail1->data<<" ";

                    tail1=tail1->next;

                }

                Node* tail2=head2->next;

                int num2=temp2->row;

                while(tail2&&tail2->row!=num2){

                    tail2=tail2->next;

                }

                while (tail2&&tail2->row==num2)

                {

                    outfile<<tail2->data<<" ";

                    tail2=tail2->next;

                }

                outfile<<endl;

            }

            temp2=temp2->next;

            }

        }

        temp1=temp1->next;

    }

    }

    outfile.close();
```

譬如
```cpp
while (temp2&&temp2->col!=y)
            {
                temp2=temp2->next;
            }
            if(temp2){
                int t2=temp2->data;
                ...
```
我一开始写的是

```cpp
while (temp2&&temp2->col!=y)
            {
                temp2=temp2->next;
            }
            int t2=temp2->data;
            ...
```

即没有检查`if(temp2)`，因为如果temp2此次为尾结点的话，虽然这次不空，但下次循环就为空(temp2=nullptr)了，然而这时我还写`int t2=temp2->data;`，当然会引发运行时错误了


> [!NOTE] 经验
> 遇到运行时错误，一定要定位到代码中有指针的地方，**一个个、一行行** 地逐个检查 **所有的指针** ，知道找出来可能为空的指针为之，一定要细心！！
