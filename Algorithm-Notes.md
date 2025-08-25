个人算法笔记
# 复习

## 小知识

* devc++编译选项`-std=c++11`
  
* 闰年：能被4整除但不能被100整除 或 能被400整除
  

    if(y%4==0&&y%100!=0||y%400==0){}

* 1既不是素数也不是合数
  
* 上取整$\lceil \frac{a}{b} \rceil=(\frac{a+b-1}{b})$
  
* `cin >> n;`返回值是`cin`本身，即`istream&`,所以可以链式输入；遇到`EOF`返回`false`
  
* `scanf`返回值为输入的个数，遇到`EOF`返回-1
  
* `getline(cin,str)`成功返回`true`，失败返回`false`，会获取前一个换行符，记得`getchar()`
  
* `puts(" ");`只输出固定的字符串（静态），且自动添加换行符`\n`
  
* 关流`ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);`，之后不能混用`cin`和`scanf`
  
* 结点的度：结点的子节点个数，即出度；树的度，结点的度的最大值。
  
* `priority_queue<int> q;`大根堆，`priority_queue<int,vector<int>,greater<int>> q;`小根堆
  

## string常用函数

    string s;
    s.find(str,pos);//从pos开始寻找第一次出现str的下标，没找到返回-1或string::npos
    s.insert(pos,str);//在pos位置插入cnt个str
    s.replace(pos,len,str);//从pos开始，将长度为len的区间替换为str
    s.erase(pos,len);
    s.append(str);// 尾部追加字符串
    s.substr(pos,len);
    s[i]=tolower(s[i]);// 转小写
    s[i]=toupper(s[i]);// 转大写
    reverse(s.beging(),s.end());// 翻转
    int a=stoi(s);
    string s=to_string(a);// 支持double
    s.clear();

## set常用函数

    s.size();
    s.insert();
    s.find();//返回迭代器，没找到返回s.end()
    s.count();//查找是否存在，存在返回1，不存在返回0，即数量
    s.erase();//值或迭代器
    //遍历
    for (auto it = s.begin(); it != s.end(); ++it) {
        cout << *it << " ";
    }
    //将 set2 的所有元素插入 set1
    set1.insert(set2.begin(), set2.end());
    // 区间删除
    set.erase(s.begin(),s.end());

## 快速幂

    int qsm(int a,int b,int mod){
        int res=1;
        while(b){
            if(b&1) res=res*a%mod;
            a=a*a%mod;
            b>>=1;
        }
        return res;
    }

## 龟速乘

    int gsc(int a,int b,int mod){
        int res=0;
        while(b){
            if(b&1) res=(res+a)%mod;
            a=2*a%mod;
            b>>=1;
        }
        return res;
    }

## 二分

* 左端点

    while(l<r){
        int mid=l+r>>1;
        if(check(mid)) r=mid;
        else l=mid+1;
    }

* 右端点

    while(l<r){
        int mid=l+r+1>>1;
        if(check(mid)) l=mid;
        else r=mid-1;
    }

## 欧几里得（最大公约数）

    int gcd(int a,int b){
        return b?gcd(b,a%b):a;
    }

## 扩展欧几里得

    int exgcd(int a,int b,int &x,int &y){
        if(!b){
            x=1,y=0;
            return a;
        }
        int x1,y1,d;
        d=exgcd(b,b%a,x1,y1);
        x=y1,y=x1-a/b*y1;
        return d;
    }

## 线性筛质数

    int pr[N],cnt;
    bool st[N];
    void get_primes(int n){
        for(int i=2;i<=n;i++){
            if(!st[i]) pr[++cnt]=i;
            for(int j=1;pr[j]*i<=n;j++){
                st[pr[j]*i]=true;
                if(i%pr[j]==0) break;
            }
        }
    }

## 逆元

* 逆元存在的前提条件：与m互质
  
* 定义：设$b$的逆元为$x$,则$a/b \equiv a \times x \pmod{m}$,$x$记为$b^{-1}$。
  
* 求：
  
  * 通法：扩展欧几里得$ax+my=gcd(a,m)=1$，求出的x即为a的逆元
    
  * m为质数时，由费马小定理$b^{m-1}\equiv 1 \pmod{m}$和$a/b \equiv a \times x \pmod{m}$得$a/b \times b^{m-1}=a \times b^{m-2}\equiv a \times x \pmod{m}$，即$x=b^{m-2}$，快速幂求解
    

## 数组模拟邻接表（链式前向星）

    int h[N],to[M],ne[M],w[M],idx;
    void add(int a,int b,int v){
        to[idx]=b,w[idx]=v,ne[idx]=h[a],h[a]=idx++;
    }

## Trie树

    int tr[N][2],idx;
    void insert(int x){
        int p=0;
        for(int i=30;i>=0;i--){
            int u=x>>i&1;
            if(tr[p][u]) p=tr[p][u];
            else{
                tr[p][u]=++idx;
                p=idx;
            }
        }
    }
    void query(int x){
        int res=0,p=0;
        for(int i=30;i>=0;i--){
            int u=x>>i&1;
            if(tr[p][u^1]){
                res=(res<<1)+1;
                p=tr[p][u^1];
            }else{
                res=(res<<1)+0;
                p=tr[p][u];
            }
        }
    }

## 树状数组模板

    int tr[N];
    int lowbit(int x){
        return x&-x;
    }
    void add(int x,int v){
        for(int i=x;i<=n;i+=lowbit(i)) tr[i]+=v;
    }
    int query(int x){
        int res=0;
        for(int i=x;i>0;i-=lowbit(i)) res+=tr[i];
        return res;
    }

## 线段树模板

    int w[N];
    struct Node{
        int l,r,sum;
    }tr[4*N];
    void pushup(int u){
        tr[u].sum=tr[u<<1].sum+tr[u<<1|1].sum;
    }
    void build(int u,int l,int r){
        if(l==r) tr[u]={l,r,w[l]};
        else{
            tr[u]={l,r};
            int mid=tr[u].l+tr[u].r>>1;
            build(u<<1,l,mid),build(u<<1|1,mid+1,r);
            pushup(u);
        }
    }
    void query(int u,int a,int b){
        if(a<=tr[u].l&&tr[u].r<=b) return tr[u].sum;
        int mid=tr[u].l+tr[u].r>>1;
        int res=0;
        if(a<=mid) res+=query(u<<1,a,b);
        if(b>mid) res+=quert(u<<1|1,a,b);
        return res;
    }
    void modify(int u,int x,int v){
        if(tr[u].l==tr[u].r) tr[u].sum=v;
        else{
            int mid=tr[u].l+tr[u].r>>1;
            if(x<=mid) modify(u<<1,x,v);
            else modify(u<<1|1,x,v);
            pushup(u);
        }
    }

## 并查集

    int p[N];
    int find(x){
        if(p[x]!=x) p[x]=find(p[x]);
        return p[x];
    }

## Dijkstra

每次找到队列中最短的出队，就已确定，每个点只枚举一次子节点更新

    struct Edge{
        int v,w;
    };
    vector<Edge> e[N];
    int dis[N];
    bool st[N];
    priority_queue<pair<int,int>> q;
    void dijkstra(int s){
        memset(dis,0x3f,sizeof(dis));
        dis[s]=0;q.push({0,s});
        while(!q.empty()){
            auto t=q.top();q.pop();
            int u=t.second;
            if(st[u]) continue;
            st[u]=true;
            for(auto ed:e[u]){
                int v=ed.v,w=ed.w;
                if(dis[v]>dis[u]+w){
                    dis[v]=dis[u]+w;
                    q.push({-dis[v],v});
                }
            } 
        }
    }

## spfa

类似bfs，相比Bellman-For关键优化是避免对同一个结点进行不必要的重复松弛，后续的松弛操作会基于当前最新的 dist[u] 进行，因此不需要重复入队。

一个点最多经过n-1条边，如果cnt>=n则证明有负环

    struct Edge{
        int v,w;
    };
    vector<Edge> e[N];
    int dis[N],cnt[N];
    bool st[N];
    queue<int> q;
    bool spfa(int s){
        memset(dis,0x3f,sizeof(dis));
        dis[s]=0;q.push(s);
        while(!q.empty()){
            int u=q.front();q.pop();
            st[u]=false;
            for(auto ed:e[u]){
                int v=ed.v,w=ed.w;
                if(dis[v]>dis[u]+w){
                    dis[v]=dis[u]+w;
                    cnt[v]=cnt[u]+1;
                    if(cnt[v]>=n) return true;
                    if(!st[v]) q.push(v),st[v]=true;
                }
            }
        }
        return false;
    }

## Floyd

    int dis[N][N];
    void floyd(){
        for(int k=1;k<=n;k++){
            for(int i=1;i<=n;i++){
                for(int j=1;j<=n;j++){
                    dis[i][j]=min(dis[i][j],dis[i][k]+dis[k][j]);
                }
            }
        }
    }
    int main(){
        memset(dis,0x3f,sizeof(dis));
        for(int i=1;i<=n;i++) dis[i][i]=0;
        for(int i=1;i<=n;i++){
            cin >> a >> b >> w;
            dis[a][b]=min(dis[a][b],w);
        }
        floyd();
        for(int i=1;i<=n;i++){
            if(dis[i][i]>inf/2) 不可达;   
        }
        for(int i=1;i<=n;i++){
            if(dis[i][i]<0) 有负环;   
        }
        return 0;
    }

## Kruskal

    int p[x];
    struct Edge{
        int x,y,w;
        bool operator < (const Edge &t){
            return w<t.w;
        }
    }e[N];
    // void add()...
    int find(x){
        if(p[x]!=x) p[x]=find(p[x]);
        return p[x];
    }
    void kruskal(){
        sort(e,e+n);
        for(int i=1;i<=n;i++) p[i]=i;
        for(int i=0;i<n;i++){
            int a=e[i].x,b=e[i].y,v=e[i].w;
            int pa=find(a),pb=find(b);
            if(pa!=pb){
                add(a,b,v),add(b,a,v);
                p[pa]=pb;    
            }
        }
    }

## 最近公共祖先（LCA）

* 倍增法

    vector<int> g[N];
    int d[N],f[N][20];
    void dfs(int u,int fa){
        d[u]=d[f]+1,f[u][0]=fa;
        for(int i=1;i<=19;i++) f[u][i]=f[f[u][i-1]][i-1];
        for(auto v:g[u]){
            if(v!=fa) dfs(v,u);
        }
    }
    int lca(int u,int v){
        if(d[u]<d[v]) swap(u,v);
        for(int i=19;i>=0;i--){
            if(d[f[u][i]]>=d[v]) u=f[u][i];    
        }
        if(u==v) return u;
        for(int i=19;i>=0;i--){
            if(f[u][i]!=f[v][i]){
                u=f[u][i],v=f[v][i];
            }    
        }
        return f[u][0];
    }

## 高斯消元（约旦消元法）

    double a[N][N];
    int gauss_jordan(){
        int c,r;
        for(c=1,r=1;c<=n;c++){
            int t=r;
            while(fabs(a[t][c])<eps&&t<n) t++;
            if(fabs(a[t][c])<eps) continue;
            swap(a[t],a[r]);
            // 对角化
            for(int i=1;i<=n;i++){
                if(i==r) continue;
                double =a[i][c]/a[r][c];
                for(int j=c;j<=n+1;j++){
                    a[i][j]-=a[r][j]*k;
                }
            }
            r++;
        }
        if(r<n+1){
            for(int i=r;i<=n;i++){
                if(fabs(a[i][n+1])>eps) return 0;
            }
            return 2;
        }
        for(int i=1;i<=n;i++){
            a[i][n+1]/=a[i][i];
        }
        return 1;
    }

## 离散化

    vector<int> alls; // 存储所有待处理点
    sort(alls.begin(),alls.end()); // 排序
    alls.erase(unique(alls.begin(),alls.end()),alls.end()); // 去重
    // 用find()寻找映射值
    // 用二分法, 到第一个大于等于x的位置
    int find1(int x){
        int l=0,r=alls.size()-1;
        while(l<r){
            int mid=l+r>>1;
            if(alls[mid]>=x) r=mid;
            else l=mid+1;
        }
        return r+1; // 映射到1,2,3,...
    }
    // 用lower_bound(),找到第一个大于等于x的位置(其实也是二分)
    int find2(int x){
        return lower_bound(alls.begin(),alls.end(),x);
    }
