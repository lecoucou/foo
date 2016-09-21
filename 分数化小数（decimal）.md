> 习题2-5　分数化小数（decimal）
> 输入正整数a，b，c，输出a/b的小数形式，精确到小数点后c位。a，b≤106，c≤100。输入包含多组数据，结束标记为a＝b＝c＝0。
>
> 样例输入：
> 1 6 4
> 0 0 0
> 
> 样例输出：
> Case 1: 0.1667

```C++
#include <stdio.h>

int main(void)
{
	int a, b, c;
	
	scanf("%d%d%d", &a, &b, &c);
	
	double s = (double)a / b;
	printf("%.*f\n", c, s);
}
```

用到了printf的'*'用法（说明符后置）。
