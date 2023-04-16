# **Bài 1:Task**
## **Tóm tắt**:

#### Có $n$ học sinh và $k$ đề .Phát đề theo thứ tự từ trái qua phải từ trên xuống dưới .Mỗi bàn có $2$ học sinh. Học sinh $1$ ngồi ở vị trí hàng $p$ cột $q$
#### Để cùng đề với học sinh $1$ học sinh $2$ phải ngồi ở vị trí nào ưu tiên ngồi trước

- $1 \le n \le 10^{5}$ .
- $2 \le k \le n$.
- $1 \le p \le (n+1)/2$.
- $1 \le q \le 2$.
## **+Thuật toán**:
#### Ta sẽ tìm xem thứ tự ngồi của học sinh $1$ là thứ tự thứ mấy nếu ta đi từ đầu lớp học xuống vị trí đấy theo thứ tự từ $trái$ qua $phải$ từ $trên$ xuống $dưới$.
#### Gọi thứ tự này là $value$.Khi đó vị trí gần nhất cũng có đề $k$ là $value-k$ và $value+k$.Ta chỉ cần xác định $hàng$ và $cột$ của từng vị trí 
## **CODE**: 
```
    int value=(p-1)*2+(q);
    if(value-k<=0&&value+k>n)
    {
        cout<<-1;
        return 0;
    }
    ii ans=make_pair(-1,-1);
    if(value>k)
    {
        int res=value-k;
        ans.x=res/2+res%2;
        ans.y=(res-(ans.x-1)*2);
    }
    if(value+k<=n)
    {
        int res=value+k;
        int used=res/2+res%2;
        if(used-p<p-ans.x||ans.x==-1)
        {
            ans.x=used;
            ans.y=(res-(ans.x-1)*2);
        }
    }
```


