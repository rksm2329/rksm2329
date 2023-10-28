# My Intro
![](https://github-readme-stats.vercel.app/api?username=rksm2329&hide_title=true&hide_border=true&show_icons=trueline_height=21&text_color=000&icon_color=000&bg_color=0,ea6161,ffc64d,fffc4d,52fa5a&theme=graywhite)

![](https://github-readme-stats.vercel.app/api/top-langs/?username=rksm2329&hide_title=true&hide_border=true&layout=compact&langs_count=6&text_color=000&icon_color=fff&bg_color=0,52fa5a,4dfcff,c64dff&theme=graywhite)

![](https://github-profile-trophy.vercel.app/?username=rksm2329)

![](https://activity-graph.herokuapp.com/graph?username=rksm2329&theme=xcode)

![](https://github-readme-streak-stats.herokuapp.com/?user=rksm2329)
# My Project: [bigInt.hpp](https://github.com/rksm2329/heyx-code-respiratory/edit/main/bigInt.hpp)
### Code
```cpp
#include <bits/stdc++.h>

using namespace std;

namespace Integer {
const string symbol = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
template <size_t kMaxLength, int binary, typename T> struct BigInt {
  T n, a[kMaxLength];
  bool f;
  BigInt() { Init(); }
  T &operator[](int i) { return a[i]; }
  void Init() {
    n = f = 1;
    fill(a, a + kMaxLength, 0);
  }
  friend istream &operator>>(istream &tempStream, BigInt &a) {
    string s;
    tempStream >> s;
    a.n = s.size();
    if (s[0] == '-') {
      a.f = 0;
      a.n--, s.erase(0, 1);
    }
    for (int i = 0; i < a.n; i++) {
      a[i] = (s[a.n - i - 1] >= 'A' && s[a.n - i - 1] <= 'Z'
                  ? s[a.n - i - 1] - 'A' + 10
                  : s[a.n - i - 1] - '0');
    }
    return tempStream;
  }
  friend ostream &operator<<(ostream &tempStream, BigInt a) {
    for (; a.n > 1 && !a[a.n - 1]; a.n--) {
    }
    if (!a.f) {
      cout << '-';
    }
    for (int i = a.n - 1; i >= 0; i--) {
      tempStream << symbol[a[i]];
    }
    return tempStream;
  }
  void operator=(int x) {
    Init();
    if (!x) {
      return;
    }
    if (x < 0) {
      x = -x, f = 0;
    }
    n = 0;
    while (x) {
      a[n++] = x % binary;
      x /= binary;
    }
  }
  void operator=(string x) {
    Init();
    int st = 0;
    if (x[0] == '-') {
      f = 0, st++;
    }
    n = 0;
    int len = x.size();
    for (int i = st; i < len; i++) {
      a[n++] = x[len - i - 1] - '0';
    }
  }
  void operator=(BigInt x) {
    Init();
    n = x.n;
    f = x.f;
    for (int i = 0; i < n; i++) {
      a[i] = x[i];
    }
  }
  bool operator==(BigInt x) {
    if (n != x.n) {
      return 0;
    }
    for (int i = n - 1; i >= 0; i++) {
      if (a[i] != x[i]) {
        return 0;
      }
    }
    return 1;
  }
  bool operator!=(BigInt x) { return !operator==(x); }
  bool operator<(BigInt x) {
    if (n == x.n) {
      for (int i = n - 1; i >= 0; i--) {
        if (a[i] != x[i]) {
          return a[i] < x[i];
        }
      }
    }
    return n < x.n;
  }
  bool operator>(BigInt x) {
    if (n == x.n) {
      for (int i = n - 1; i >= 0; i--) {
        if (a[i] != x[i]) {
          return a[i] > x[i];
        }
      }
    }
    return n > x.n;
  }
  bool operator<=(BigInt x) { return !operator>(x); }
  bool operator>=(BigInt x) { return !operator<(x); }
  bool operator!() {
    if (n != 1) {
      return 0;
    }
    return !a[0];
  }
  bool operator==(int x) {
    BigInt y;
    y = x;
    return operator==(y);
  }
  bool operator!=(int x) {
    BigInt y;
    y = x;
    return operator!=(y);
  }
  bool operator<(int x) {
    BigInt y;
    y = x;
    return operator<(y);
  }
  bool operator>(int x) {
    BigInt y;
    y = x;
    return operator>(y);
  }
  bool operator<=(int x) {
    BigInt y;
    y = x;
    return operator<=(y);
  }
  bool operator>=(int x) {
    BigInt y;
    y = x;
    return operator>=(y);
  }
  BigInt ChangeBinary(BigInt &num) {
    BigInt ans;
    ans[0] = 0, ans.n = 1;
    for (int i = 0; i < num.n; i++) {
      ans = ans * binary;
      ans = ans + num[i];
    }
    num.n = ans.n;
    return ans;
  }
  BigInt operator+(BigInt x) {
    BigInt y;
    y.n = max(n, x.n);
    int s = 0;
    for (int i = 0; i < y.n; i++) {
      s += a[i] + x.a[i];
      y.a[i] = s % binary;
      s /= binary;
    }
    for (; s; s /= binary) {
      y.a[y.n++] += s % binary;
    }
    return y;
  }
  void operator+=(BigInt x) {
    BigInt z = *this;
    *this = z + x;
  }
  void operator+=(int x) {
    BigInt z = *this;
    *this = z + x;
  }
  BigInt operator+(int x) {
    BigInt y;
    y = x;
    return operator+(y);
  }
  BigInt operator-(BigInt x) {
    BigInt y, z = *this;
    if (z < x) {
      y.f = 0, swap(z, x);
    }
    y.n = z.n;
    for (int i = 0; i < y.n; i++) {
      if (z.a[i] < x[i]) {
        z[i + 1]--;
        z[i] += binary;
      }
      y[i] = z[i] - x[i];
    }
    for (; !y[y.n - 1] && y.n > 1; y.n--) {
    }
    return y;
  }
  void operator-=(BigInt x) {
    BigInt z = *this;
    *this = z - x;
  }
  void operator-=(int x) {
    BigInt z = *this;
    *this = z - x;
  }
  BigInt operator-(int x) {
    BigInt y;
    y = x;
    return operator-(y);
  }
  BigInt operator*(BigInt x) {
    BigInt y;
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < x.n; j++) {
        y[i + j] += a[i] * x[j];
      }
    }
    y.n = n + x.n;
    for (int i = 0; i < y.n - 1; i++) {
      y[i + 1] += y[i] / binary, y[i] %= binary;
    }
    for (; !y.a[y.n - 1] && y.n > 1; y.n--) {
    }
    return y;
  }
  void operator*=(BigInt x) {
    BigInt z = *this;
    *this = z * x;
  }
  void operator*=(int x) {
    BigInt z = *this;
    *this = z * x;
  }
  BigInt operator*(int x) {
    BigInt y;
    y = x;
    return operator*(y);
  }
  BigInt operator/(BigInt x) {
    BigInt y, z = *this;
    if (x == y) {
      return x;
    }
    if (binary != 10) {
      x = ChangeBinary(x), z = ChangeBinary(z);
    }
    y.n = z.n - x.n + 1;
    for (int i = y.n - 1; i >= 0; i--) {
      BigInt t = x << i;
      for (; z >= t; z -= t) {
        y.a[i]++;
      }
    }
    for (; !y.a[y.n - 1] && y.n > 1; y.n--) {
    }
    return y;
  }
  void operator/=(BigInt x) {
    BigInt z = *this;
    *this = z / x;
  }
  void operator/=(int x) {
    BigInt z = *this;
    *this = z / x;
  }
  BigInt operator/(int x) {
    BigInt y;
    y = x;
    return operator/(y);
  }
  BigInt operator%(BigInt x) {
    BigInt z = *this;
    return z - z / x * x;
  }
  void operator%=(BigInt x) {
    BigInt z = *this;
    *this = z % x;
  }
  void operator%=(int x) {
    BigInt z = *this;
    *this = z % x;
  }
  BigInt operator%(int x) {
    BigInt y;
    y = x;
    return operator%(y);
  }
  BigInt operator<<(int l) {
    BigInt x;
    for (int i = 0; i < n; i++) {
      x[i + l] = a[i];
    }
    x.n = n + l;
    return x;
  }
  BigInt operator>>(int l) {
    BigInt x;
    x.n = n - l;
    for (int i = 0; i < x.n; i++) {
      x[i] = a[i + l];
    }
    return x;
  }
};
}; // namespace Integer
```
