# ***Bài 11 :DetectVirusin2D***
**
## **Tóm tắt**:
#### Cho một bảng $n$ * $m$ kí tự $Latin$ in thường .Một dãy các kí tự liên tiếp theo hàng hoặc theo cột tạo thành 1 xâu .Cho $q$ truy vấn.Mỗi truy vấn là xâu $s$.Có thể tao thành xâu $s$ bằng các kí tự trong bảng hay không? 
## **Ràng buộc:**
- $1 \le n*m \le 10^{5}$ .
- $2 \le Q \le 10^{4}$.
- $1 \le s.size \le 10$.
## **Thuật toán**:
### ***Thuật duyệt trâu***
#### Với mỗi vị trị ($i$ , $j$) trên bảng ta kiểm tra thử có thể tạo ra 1 xâu có độ dài $s$.size gọi là $p$ theo chiều dọc hay chiều ngang hay ko.Rồi ta so sánh với xâu $s$ có bằng $p$ hay không
### ĐPT Time *O*($2 * m * n * q$)
### **Thuật toán AC**:
#### (**1**) Việc so sánh 2 xâu 1 cách nhanh chống khiến ta nghỉ đến 1 cấu trúc dữ liệu quen thuộc là cây Trie.
#### (**2**) Với mỗi vị trí ($i$,$j$) trên bảng nếu ta thêm xâu từ vị trí ($i$ , $j$) đến ($i$ , $m$) hoặc ($n$ , $j$) vào cây ***Trie*** thì nếu xét 1 xâu $s$ bất kì thì nó sẽ là tiền tố một xâu bất kì trên cây ***Trie*** 
#### (**3**) ($1 \le s.size \le 10$)
#### Bài này ta có thể Trie theo 2 cách:
#### **Cách 1**:
- Từ (**2**) và (**3**) ta sẽ xây dựng cây ***Trie*** bằng cách tạo $n * m$ xâu theo chiều ngang và $n * m$ xâu theo chiều dọc .Với mỗi vị trí ($i$ , $j$) trên bảng ta sẽ tạo ra 1 xâu $p$ ($1 \le p \le 10$) theo thứ tự từ trái sang phải khi đó hoặc từ trên xuống dưới và thêm vào ***Trie***.
- Sau đó với mỗi truy vấn ta duyệt trên cây ***Trie*** xem thử xâu s có tồn tại hay không
- Để tối ưu thời gian tạo xâu mới khi  ta có thể sử dụng ***deque*** khi gọi trace là xâu ta tạo ở vị trí trước đó .Khi chuyển sang vị trí mới ta chỉ cần xóa kí tự ở đầu ***deque*** và thêm kí tự hiện tại vào cuối ***deque*** 
- **ĐPT Time** ($2 * n * m * 10+q*10$)
- **ĐPT Memory** ($2 * n * m * 10$)
- Vì ($1 \le n * m \le 10^{5} $) nên ta cần có 1 cây ***Trie*** $2*10^{6}$ nút mà $Memory$ $limit$  là $256ms$ nên có thể gây hết bộ nhớ ($MLE$)
#### **Cách 2**:
- Ta sẽ sử lý truy vấn ***Offline***
- Ta sẽ xây dựng cây ***Trie*** bằng cách thêm tất cả xâu truy vấn ($1 \le i \le 10^{4}$)vào ***Trie***  .Với mỗi nút trên ***Trie*** nếu nó là kết thúc của xâu đang xét ta sẽ thêm vào vector **block** thứ tự xuất hiện của xâu đang xét là **i** 
- Từ (**2**) và (**3**) Vì 1 xâu ở vị trí ($i$ , $j$) là tiền tố của $s$ nên với mỗi vị trí ($i$ , $j$) trên bảng ta sẽ tạo ra 1 xâu $p$ ($1 \le p \le 10$) theo thứ tự từ trái sang phải khi đó hoặc từ trên xuống dưới ta sẽ duyệt trên cây ***Trie*** và đánh dấu tất cả những nút mà xâu *P* đi qua bằng một mảng ***check*** .Khi đó những nút được đánh dấu mà **block** khác rõng thì chắc chắn tất cả các xâu trong **block** đó  đều tồn tại  
-  **ĐPT Time** ($2 * n * m * 10+q*10$)
-  **ĐPT Memory** ($q*10$)
- Vì ($1 \le q \le 10^{4}$) nên ta chỉ cần 1 cây ***Trie*** $10^{5}$ nút mà thôi có thể qua được
### -> Ta sẽ sử dụng cách 2 
### **ĐPT Time** ($2 * n * m * 10+q*10$)
### **ĐPT Memory** ($q * 10$)
## **CODE:**
### **Tạo cây Trie**:
```
struct trie
{
    int index[30];
    vector<int>block;
    trie()
    {
        memset(index,-1,sizeof(index));
    }
};
vector<trie>tree;
vector<int>block[M];
void add(string need,int index)
{
    int num_node=0;
    for(char c:need)
    {
        int value=c-'a';
        if(tree[num_node].index[value]==-1)
        {
            total_node++;
            tree.push_back(trie());
            tree[num_node].index[value]=total_node;
        }
        num_node=tree[num_node].index[value];
    }
    block[num_node].push_back(index);
}
```
## **Đánh dấu xâu ***S*** có tồn tại:**
```
void go(string s)
{
    int num_node=0;
    for(char c:s)
    {
        int value=c-'a';
        if(tree[num_node].index[value]==-1)return ;
        num_node=tree[num_node].index[value];
        check[num_node]=true;
    }
}
```
## **Code AC:**
```
    for(int i=1;i<=t;i++)
    {
        cin>>s[i];
        add(s[i],i);
    }
    for(int i=1;i<=n;i++)
    {
        string s;
        for(int j=1;j<=m;j++)s.push_back(c[i][j]);
        for(int j=1;j<=m;j++)
        {
            go(s);
            s.erase(s.begin());
        }
    }
    for(int j=1;j<=m;j++)
    {
        string s;
        for(int i=1;i<=n;i++)s.push_back(c[i][j]);
        for(int i=1;i<=n;i++)
        {
            go(s);
            s.erase(s.begin());
        }
    }
    vector<int>ans(t+1);
    for(int i=1;i<=total_node;i++)
    {
        if(!check[i])continue;
        for(int node:block[i])ans[node]=1;
    }
    for(int i=1;i<=t;i++)cout<<ans[i];
```

 
 
 



