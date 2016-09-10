广州市第二中学高一8班颜汇杭，有C/C++/Haskell/Perl/Scheme/Common Lisp及算法基础，非常熟悉C标准库（C89），有着很好的编码风格（但是有时候注释写的比较少），还会使用Markdown/HTML撰写文档，熟悉VS/DevC++/Emacs/Vim/Qt Creator，会用Github找各种库，会用Linux(Debian)，英语、数学成绩优异，学习过高等数学。希望能参加信息学竞赛。

以下是一些以前的作品，全部都是完完全全自己实现的，可以点击超链接进入博客查看，也可以在下面直接查看：


* **Haskell** [在一个网站上做的题集][H1]

[H1]: http://www.cnblogs.com/jt2001/p/projectEuler.html

* **ECMAScript(JS)** [前缀表达式（Lisp）转函数表达式（C-Like）][E1]

[E1]:http://www.cnblogs.com/jt2001/p/js_scheme2eigenmath.html

* **C语言** [八皇后问题][C1]

[C1]:http://www.cnblogs.com/jt2001/p/5179487.html

* **Eigenmath** [二分法与牛顿-拉夫森迭代][E2]

[E2]:http://www.cnblogs.com/jt2001/p/math---How-to-solve-fx-equal-to-0---Newton-VS-bisection.html

* **C++** [单链式结构的深拷贝][CP1]

[CP1]:http://www.cnblogs.com/jt2001/p/sldc20150811.html

* **C++** [实现队列（queue）容器类][CP2]

[CP2]:http://www.cnblogs.com/jt2001/p/queue20150811.html
 
* **C++** [实现栈(stack)容器类][CP3]

[CP3]:http://www.cnblogs.com/jt2001/p/stack20150810.html

* **C++** 扫雷

这个找不到了。用Qt库实现的。游戏逻辑很简单，主要就是写生成地图、鼠标事件和绘图。

* **C++11** [模仿STL的list容器类实现双链表][CP4]

[CP4]:http://www.cnblogs.com/jt2001/p/cpplist20150819.html

* **C++11** [背包问题][CP5]

[CP5]:http://www.cnblogs.com/jt2001/p/packquestionall.html


# Haskell 在一个网站上做的题集

小标题是自己根据引文加的，引文部分为题目原文。所有在这里列出的题目都经过该网站的验证 ，都是正确的。

## 在1000位大数里面找13个数字，使它们乘积最大
> The four adjacent digits in the 1000-digit number that have the greatest product are 9 × 9 × 8 × 9 = 5832.
> 
> 73167176531330624919225119674426574742355349194934
> 96983520312774506326239578318016984801869478851843
> 85861560789112949495459501737958331952853208805511
> 12540698747158523863050715693290963295227443043557
> 66896648950445244523161731856403098711121722383113
> 62229893423380308135336276614282806444486645238749
> 30358907296290491560440772390713810515859307960866
> 70172427121883998797908792274921901699720888093776
> 65727333001053367881220235421809751254540594752243
> 52584907711670556013604839586446706324415722155397
> 53697817977846174064955149290862569321978468622482
> 83972241375657056057490261407972968652414535100474
> 82166370484403199890008895243450658541227588666881
> 16427171479924442928230863465674813919123162824586
> 17866458359124566529476545682848912883142607690042
> 24219022671055626321111109370544217506941658960408
> 07198403850962455444362981230987879927244284909188
> 84580156166097919133875499200524063689912560717606
> 05886116467109405077541002256983155200055935729725
> 71636269561882670428252483600823257530420752963450
> 
> Find the thirteen adjacent digits in the 1000-digit number that have the greatest product. What is the value of this product?

```Haskell
import Data.Char
ans = maximum productLst
 where productLst = getProductLst number
        where number = "7316717653133062491922511967442657474235534919493496983520312774506326239578318016984801869478851843858615607891129494954595017379583319528532088055111254069874715852386305071569329096329522744304355766896648950445244523161731856403098711121722383113622298934233803081353362766142828064444866452387493035890729629049156044077239071381051585930796086670172427121883998797908792274921901699720888093776657273330010533678812202354218097512545405947522435258490771167055601360483958644670632441572215539753697817977846174064955149290862569321978468622482839722413756570560574902614079729686524145351004748216637048440319989000889524345065854122758866688116427171479924442928230863465674813919123162824586178664583591245665294765456828489128831426076900422421902267105562632111110937054421750694165896040807198403850962455444362981230987879927244284909188845801561660979191338754992005240636899125607176060588611646710940507754100225698315520005593572972571636269561882670428252483600823257530420752963450"
              getProductLst l | len == 13   = [ productOf1to13 ]
                              | otherwise   = productOf1to13:getProductLst(tail l)
               where len = length l
                     productOf1to13 = product from1to13
                      where from1to13 = map (\ a -> read (a:[]) :: Integer) $ take 13 l
--                                                                 ^^^^^^^
--                                           Necessary. Or you will get some negative numbers.
```

大坑。调了好久，原来是溢出了。


## 4m以内偶数斐波那契数求和
> Each new term in the Fibonacci sequence is generated by adding the previous two terms. By starting with 1 and 2, the first 10 terms will be:
> 
> 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...
> 
> By considering the terms in the Fibonacci sequence whose values do not exceed four million, find the sum of the even-valued terms.

```Haskell
answer = sum $ filter even $ takeWhile (<= 4000000) $ map fst $
                                                               iterate (\ (a,b) -> (b, a+b))
                                                                       (1, 2)
```

## 大数因式分解
> The prime factors of 13195 are 5, 7, 13 and 29.
> What is the largest prime factor of the number 600851475143 ?

```Haskell
answer = maximum $ fact 600851475143
    where
        -- Get the factors of N, not sorted
        fact n = factors n 2
            where
                -- Get the factors of N
                -- p <= (the next factor of n),  p <- Z+
                factors n p | n == 1     = [1]
                            | n  > 1     = if    mod n p == 0              -- if    p is a factor
                                           then  p:(factors (div n p) p)
                                           else  factors n (succ p)        -- else  try next
```

## 找数值上最大的6位回文数
> A palindromic number reads the same both ways. The largest palindrome made from the product of two 2-digit numbers is 9009 = 91 × 99.
> Find the largest palindrome made from the product of two 3-digit numbers.

```Haskell
answer = maximum $ [ x*y | x <- [ 100 .. 999 ],
                           y <- [ 100 .. 999 ],
                           isPalindrome $ show(x*y)
                   ]
                   where
                       isPalindrome l = l == reverse(l)
```

## 穷举找毕德哥拉斯三元组使得和为1000
> A Pythagorean triplet is a set of three natural numbers, a < b < c, for which,
> a^2 + b^2 = c^2
> 
> For example, 3^2 + 4^2 = 9 + 16 = 25 = 5^2.
> 
> There exists exactly one Pythagorean triplet for which a + b + c = 1000.
> Find the product abc.
```Haskell
ans = product' $ head $ [ (a, b, c) | c <- [ 1 .. 1000 ],
                                      b <- [ 1 .. c ],
                                      a <- [ 1 .. b ],
                                      a + b + c == 1000,
                                      a^2 + b^2 == c^2
                        ]
 where product' (x, y, z) = x * y * z
```

## 找1000以内有3和5因子的数
> If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.
> Find the sum of all the multiples of 3 or 5 below 1000.

```Haskell
answer = sum $ takeWhile (< 1000) [ n | n <- [ 1 .. 999 ],
                                        mod n 3 == 0 || mod n 5 == 0
                                  ]
```





# C++11 双链表


这个双链表，是模仿stl的list制作的，只实现了一些基本功能，像merge，transfer这些就没有实现，可以继承一下，用基本操作来自己做外部实现。

没有选用stl的[begin,end)迭代器模式，而是使用传统的[head,tail]。不过，为了配合stl算法，还是加了两个begin()，end()方法，模拟了一下stl容器。模拟的还算及格，至少可以类似for (; begin != end; ++begin)这样的事，也可以让容器搭配一些stl算法，这在之后的demo里可以看到。不过，的iterator没有traits，所以不能用于std::sort。

至于模拟的实现，方法是让所有越界的迭代器都指向一个预先设定好的内存地址（野指针）。然后用户调用end()方法的时候，就返回指向这个内存地址的迭代器。这样，当迭代器越界的时候，就和end()指向的内存地址一样了。

```C++
#pragma once

#include <cstddef>
#include <stdexcept>

namespace jt {

template <class value_t>
class list {
  friend class list<value_t>;

private:
  struct node {
    value_t val;
    node *next, *prev;
  } *_head = nullptr,
    *_tail = nullptr;

  size_t _siz;

  enum { endpointer = 1 };

public:
  class iterator {
    friend class iterator;
    friend class list<value_t>;

  public:
    iterator(node *nn = nullptr) { _init(nn); }
    iterator(const iterator &i)  { _init(i.n); }

    value_t& operator*() const { return n->val; }
    value_t* operator->() const { return &(operator*()); }
    iterator operator++() { _inc(); return *this; }
    iterator operator++(int) { iterator tmp = *this; _inc(); return tmp; }
    iterator operator--() { _dec(); return *this; }
    iterator operator--(int) { iterator tmp = *this; _dec(); return tmp; }
    bool operator==(iterator& i) const { return i.n == n; }
    bool operator!=(iterator& i) const { return i.n != n; }

  private:
    node *n;

    void _init(node *nn) {
      if (nn) {
        n = nn;
      } else {
        n = (node*)endpointer;
      }
    }

    void _inc() { n = (n->next) ? n->next : (node*)endpointer; } // increment
    void _dec() { n = (n->prev) ? n->prev : (node*)endpointer; } // decrement
  };

  list() { clear(); }
  list(const list<value_t> &l) { if (&l != this) _init(l.begin(), l.end()); }
  ~list() { clear(); }

  bool empty() const { return !_head; }

  void clear() {
    if (!empty()) {
      node *tmp;
      for (node *n = _head; n; n = tmp) {
        tmp = n->next;
        delete n;
      }

      _head = nullptr;
      _tail = nullptr;
      _siz = 0;
    }
  }

  void insert(const iterator &pos, const value_t &val) {
    if (pos.n == _head) {
      push_front(val);
    } else {
      node *n = new node;
      n->val = val;
      n->next = pos.n;
      n->prev = pos.n->prev;

      pos.n->prev->next = pos.n->prev = n;
      ++_siz;
    }
  }

  void erase(const iterator &pos) {
    if (pos.n == _head) {
      pop_front();
    } else if (pos.n == _tail) {
      pop_back();
    } else {
      pos.n->prev->next = pos.n->next;
      pos.n->next->prev = pos.n->prev;
      delete pos.n;
      --_siz;
    }
  }

  iterator head() const { return iterator(_head); }
  iterator tail() const { return iterator(_tail); }

  iterator begin() const { return iterator(_head); }
  iterator end() const { return iterator((node*)endpointer); }

  void push_front(const value_t &val) {
    node *n = new node;
    n->val = val;
    n->next = _head;
    n->prev = nullptr;

    if (empty()) _tail = n;
    else _head->prev = n;
    _head = n;

    ++_siz;
  }

  void push_back(const value_t &val) {
    node *n = new node;
    n->val = val;
    n->next = nullptr;
    n->prev = _tail;

    if (empty()) _head = n;
    else _tail->next = n;
    _tail = n;

    ++_siz;
  }

  void pop_front() {
    if (_siz == 0) {
      throw std::logic_error("Empty List");
    } else if (_siz == 1) {
      clear();
    } else {
      node *tmp = _head->next;
      delete _head;
      _head = tmp;
      _head->prev = nullptr;
    }

    --_siz;
  }

  void pop_back() {
    if (_siz == 0) {
      throw std::logic_error("Empty List");
    } else if (_siz == 1) {
      clear();
    } else {
      node *tmp = _tail->prev;
      delete _tail;
      _tail = tmp;
      _tail->next = nullptr;
    }

    --_siz;
  }

  value_t front() const { return *head(); }
  value_t back()  const { return *tail(); }

  size_t size() const { return _siz; }

private:
  template <class iter>
  void _init(iter head, iter tail) {
    clear();

    // copy
    if (head && tail) {
      for (; head != tail; ++head) {
        push_back(*head);
      }
      push_back(*tail);
    }
  }
};

}
```


# C++ 单链式结构的深拷贝

当时看STL源码，发现都用两个空格作tab，自己也学着做。后来发现看得很累，又改成8个了。

```C++
template <class value>
void sldc(node<value> **dst, const node<value> *src) { // single list deep copy
  while (*dst) { // clear
    auto **tmp = &(*dst)->next;
    delete tmp;
    dst = tmp;
  }
  *dst = nullptr;

  for (; src; src = src->next, dst = &(*dst)->next) {
    *dst = new in;
    (*dst)->val = src->val;
  }
}
```

# C++ 队列queue容器类

同一时期作品。受到STL的影响，标识符都写得非常短，还喜欢写_impl。namespace jt是当时的网名。

```C++
#pragma once
#include <cstddef>
#include <stdexcept>

namespace jt {

template <class value>
class queue {
  template <class v>
  friend void _queue_copy(queue<v>& dst, const queue<v>& src);

private:
  struct _queue_impl;
  _queue_impl *_impl = nullptr;
  size_t _siz;

  void _init_empty() {
    _siz = 0;
    if (_impl) delete _impl;
    _impl = new _queue_impl;
  }

  void _destory() { _siz = 0; delete _impl; }

public:
  queue() { _init_empty(); }
  queue(const queue<value> &o) { _queue_copy(*this, o); }
  ~queue() { _destory(); }

  void clear() { _init_empty(); }
  void push(const value &val) { _impl->push(val); ++_siz; }
  size_t size() const { return _siz; }

  value pop() {
    if (_siz == 0)
      throw std::out_of_range("jt::queue::pop() - Empty queue");
    --_siz; return _impl->pop();
  }

  bool empty() const { return _siz == 0; }

  value front() const {
    if (_siz == 0)
      throw std::out_of_range("jt::queue::front() - Empty queue");
    return _impl->f->val;
  }

  value back() const {
    if (_siz == 0)
      throw std::out_of_range("jt::queue::back() - Empty queue");
    return _impl->b->val;
  }
};

template <class value>
static void _queue_copy(queue<value> &dst, const queue<value> &src) {
  dst._init_empty();
  auto **dn = &dst._impl->f; // dest node

  for (auto *s = src._impl->f; s; s = s->next) {
    *dn = new typename queue<value>::_queue_impl::node;
    (*dn)->val = s->val;
    dst._impl->b = *dn;
    dn = &(*dn)->next;
  }

  dst._siz = src._siz;
}

template <class value>
struct queue<value>::_queue_impl {
  struct node {
    value val;
    node *next = nullptr;
  } *f = nullptr, // front
    *b;           // back

  ~_queue_impl() {
    while (f) {
      auto *tmp = f->next;
      delete f;
      f = tmp;
    }
  }

  void push(const value& val) {
    node *t = new node;
    t->val = val;

    if (f) b->next = t;
    else f = t; // if empty, set front

    b = t;
  }

  value pop() {
    value v = f->val;

    node *tmp = f->next;
    delete f;
    f = tmp;

    return v;
  }
};

}
```
 
# C++ 栈(stack)容器类

同一时期作品。之后觉得抛异常写string麻烦就直接assert(0)了。

```C++
#pragma once
#include <cstddef>
#include <stdexcept>

namespace jt {

template <class value>
class stack {
  template <class v>
  friend void _stack_copy(stack<v>& dst, const stack<v>& src);

private:
  struct _stack_impl;
  _stack_impl *_impl = nullptr;
  size_t _siz;

  void _init_empty() {
    _siz = 0;
    if (_impl) delete _impl;
    _impl = new _stack_impl;
  }

  void _destory() { _siz = 0; delete _impl; }

public:
  stack() { _init_empty(); }
  stack(const stack<value> &o) { _stack_copy(*this, o); }
  ~stack() { _destory(); }

  void clear() { _init_empty(); }
  void push(const value &val) { _impl->push(val); ++_siz; }
  size_t size() const { return _siz; }

  value pop() {
    if (_siz == 0)
      throw std::out_of_range("jt::stack::pop() - Empty Stack");
    --_siz; return _impl->pop();
  }

  bool empty() const { return _siz == 0; }

  value top() const {
    if (_siz == 0)
      throw std::out_of_range("jt::stack::top() - Empty Stack");
    return _impl->n->val;
  }
};

template <class value>
static void _stack_copy(stack<value> &dst, const stack<value> &src) {
  dst._init_empty();
  auto **dn = &dst._impl->n; // dest node

  for (auto *s = src._impl->n; s; s = s->next) {
    *dn = new typename stack<value>::_stack_impl::node;
    (*dn)->val = s->val;
    dn = &(*dn)->next;
  }

  dst._siz = src._siz;
}

template <class value>
struct stack<value>::_stack_impl {
  struct node {
    value val;
    node *next = nullptr;
  } *n = nullptr; // head/top

  ~_stack_impl() {
    while (n) {
      auto *tmp = n->next;
      delete n;
      n = tmp;
    }
  }

  void push(const value &val) {
    node *t = new node;
    t->val = val;
    t->next = n;

    n = t;
  }

  value pop() {
    value v = n->val;

    node *tmp = n->next;
    delete n;
    n = tmp;

    return v;
  }
};

}
```




# C++ 背包问题

感谢匿名函数！

```C++
#include <iostream>
#include <functional>

struct Pack {
  unsigned cnt;
  unsigned *w; // weights
  unsigned *v; // values
  unsigned *x; // put in or not
  unsigned ca; // capacity
  
  Pack(unsigned items_cnt) : cnt(items_cnt) {
    w = new unsigned [items_cnt];
    v = new unsigned [items_cnt];
    x = new unsigned [items_cnt];
    
    for (int i = 0; i < items_cnt; ++i) {
      w[i] = v[i] = x[i] = -1;
    }
  }
  
  ~Pack() {
    delete [] w;
    delete [] v;
    delete [] x;
  }
};


int main() {
  unsigned c;
 
  std::cin >> c;
  
  Pack p(c);
  
  std::cin >> p.ca;
  
  for (int i = 0; i < p.cnt; ++i) {
    std::cin >> p.w[i] >> p.v[i];
  }
 
 
 
 
  std::function<unsigned()> totval = [&]() {
    unsigned cnt = 0;
    
    for (int i = 0; i < p.ca; ++i) {
      if (p.x[i] == 1) cnt += p.v[i];
    }
    
    return cnt;
  };
  
  std::function<unsigned()> totwt = [&]() {
    unsigned cnt = 0;
    
    for (int i = 0; i < p.ca; ++i) {
      if (p.x[i] == 1) cnt += p.w[i];
    }
    
    return cnt;
  };
  
  std::function<void()> loop = [&]() {
    unsigned no = -1;
    for (int i = 0; i < p.cnt; ++i) {
      if (p.x[i] == -1) {
        no = i;
        break;
      }
    }
    
    if (no == -1) {
      std::cout << totval() << ' ';
      for (int i = 0; i < p.cnt; ++i) {
        std::cout << p.x[i];
      }
      std::cout << std::endl;
    } else {
      p.x[no] = 0;
      loop();
      
      p.x[no] = 1;
      
      if (totwt() <= p.ca) {
        loop();
      }
      
      p.x[no] = -1;
    }
  };
  
  loop();
 
  return 0;
}
```



# Eigenmath 二分法与牛顿-拉夫森迭代

第一次写Eigenmath。这是注释写得最多的一次了。

## 二分法

```
f(x) = 3^(x+1)+x^3                       
draw(f)
l=-10                               # low
h=0                                 # high
cnt=0                               # count, 记录迭代次数
error=10^(-5)                       # 误差。若abs(high - low)<error则输出结果
for(k,1,10^9,                       # 无限循环，直到小于误差
    do(
        cnt=cnt+1,                  # 记录迭代次数
        m=(l+h)/2,                  # 取二分点m
        test(
            f(m)>0,                 # 若f(m)>0
            h=m,                    #   h=m
            l=m                     # 否则 l=m
        ),
        test(
            abs(h-l) > error,       # 若结果大于误差
            1,                      #   什么都不做，继续迭代
            do(                     # 否则
                print(
                    float(l),       #   输出low（小数形式）
                    float(h),       #   输出high（小数形式）
                    cnt             #   输出迭代次数
                ),
                stop                #   退出迭代
            )
        )
    )
)
```

## 牛顿法

Quake 3源码里约翰卡马克的那个Magic Number就是牛顿法。

```
f(x) = 3^(x+1)+x^3
draw(f)
guess=0                                                         # 猜测的x0的值
cnt=0                                                           # 记录迭代次数（同上）
error=10^(-5)                                                   # 误差（同上）
for(k,1,10^9,                                                   # 无限循环（同上）
    do(
        cnt=cnt+1,                                              # 记录迭代次数（同上）
        x1=float(guess- f(guess)/eval(d(f,x),x,guess)),         # 使用牛顿法公式得出一个比guess更接近x0的值
        test( abs(x1-guess) > error,                            # 若猜测值增量大于误差（表示还没猜到）
            guess=x1,                                           #   更新猜测值guess，继续迭代
            do(                                                 # 否则
                print(x1, cnt),                                 #   输出接近x0的猜测值与迭代次数
                stop                                            #   退出迭代
            )
        )
    )
)
```


# C语言 八皇后问题

简单的穷举。

```C
/*
 * 阅读代码顺序：从下至上
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MARK(r, c) do { \
    if (newBoard[r][c] == AVAILABLE) { \
        newBoard[r][c] = UNSAFE; \
    } \
} while (0)

#define CHECK_AND_MARK(r, c) do { \
    if (0 <= r && r < 8 && 0 <= c && c < 8 && \
        newBoard[r][c] == AVAILABLE) { \
            newBoard[r][c] = UNSAFE; \
    } \
} while (0)

/*
 * 3: queens函数的辅助宏
 */

/*
 * 储存某一时刻的棋盘信息
 */
enum {
    QUEEN     = 'X', // 放了皇后
    AVAILABLE = '_', // 没放皇后但可以放
    UNSAFE    = '.'  // 没放皇后且不能放
};
typedef char Chessboard[8][8];

void queens(Chessboard board, unsigned n, unsigned *total) {
/*     在棋盘board上，从第n个皇后枚举到第8个皇后，将解法总和加到total上
 *     如果遇到解法，则输出
 */
    if (n == 9) { // 输出
        int r, c;
        for (r = 0; r < 8; ++r) {
            for (c = 0; c < 8; ++c) {
                printf("%s%c%s",
                       (c == 0) ? "|" : "",
                       board[r][c],
                       (c == 7) ? "|\n" : "|");
            }
        }
        printf("\n");        
        
        ++*total;
    } else {
        int r, c;

        for (r = n - 1; r < 8; ++r) {
            for (c = 0; c < 8; ++c) {
                if (board[r][c] != AVAILABLE) continue;
                
                Chessboard newBoard;
                memcpy(newBoard, board, sizeof(Chessboard));
                newBoard[r][c] = QUEEN;

                int rr, cc, i;
                for (cc = 0; cc < 8; ++cc) { // 行
                    MARK(r, cc); // 标上UNSAFE
                }
                for (rr = 0; rr < 8; ++rr) { // 列
                    MARK(rr, c);
                }
                for (i = 1; i < 8; ++i) { // 对角线
                    // 若点(x,y)与点(x',y')处于同一对角线，则
                    // |x'-x| = |y'-y|
                    CHECK_AND_MARK(r + i, c + i); // 假如坐标合法，标UNSAFE
                    CHECK_AND_MARK(r + i, c - i);
                    CHECK_AND_MARK(r - i, c + i);
                    CHECK_AND_MARK(r - i, c - i);
                }

                queens(newBoard, n + 1, total);
            }
        }
    }
}

/*
 * 2: queens函数及Chessboard类型
 */

int main(int argc, char **argv) {
    Chessboard board;
    memset(board, AVAILABLE, sizeof(Chessboard));
    
    unsigned total = 0;
    
    queens(board, 1, &total);
    
    printf("Total: %d\n", total);

    exit(0);
}

/*
 * 1: Main
 */
```



# ECMAScript(JS) 前缀表达式（Lisp）转函数表达式（C-Like）

只列出转换的算法部分，其他部分可以到博客上看。

```JavaScript
    function enter(r, o) {
        var root='me';
        function Node(m) {
            this.ma = m;
            this.v = [];
            this.toString = function(){
                var s = this.v.shift();
                s += '(';
                for (var i in this.v) {
                    if (i != 0) s += ',';
                    s += this.v[i].toString();
                }
                s += ')';
                return s;
            };
        }

        var node=new Node(root);
        var s='';
        r=r.split('');

        for (var i in r) {
            switch (r[i]) {
                case '(':
                    var n = new Node(node);
                    node.v.push(n);
                    node = n;
                    break;
                case ')':
                    if (s != '') {
                        node.v.push(s);
                        s = '';
                    }
                    node = node.ma;
                    break;
                case '\n':
                case '\t':
                case ' ':
                    if (s != '') {
                        node.v.push(s);
                        s = '';
                    }
                    break;
                default:
                    s = s + r[i];
            }
        }
        node = node.v[0];

        o.value = node.toString();
    }
```
