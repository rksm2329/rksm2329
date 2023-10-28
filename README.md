# `bigInt.h`
```cpp
#include <iostream>

using namespace std;

namespace Int {
using namespace std;
const string binary_symbol = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
template <size_t Maxlen = 1000 + 3, class T1 = int> struct BigInt {
  int len = 0, binary = 10, pm = 1;
  T1 num[Maxlen] = {0};
  BigInt() {}
  BigInt(const string &s) {
    len = s.size();
    for (int i = 0; i < len; i++) {
      num[i] = s[len - i - 1] - '0';
    }
  }
  BigInt(const BigInt &b) {
    len = b.len;
    for (int i = 0; i < len; i++) {
      num[i] = b.num[i];
    }
  }
  friend istream &operator>>(istream &is, BigInt &a) {
    string s;
    is >> s;
    a.len = s.size();
    if (s[0] == '-') {
      a.pm = -1;
      a.len--, s.erase(0, 1);
    }
    for (int i = 0; i < a.len; i++) {
      if (s[a.len - i - 1] >= 'A' && s[a.len - i - 1] <= 'Z') {
        a[i] = s[a.len - i - 1] - 'A' + 10;
      } else {
        a[i] = s[a.len - i - 1] - '0';
      }
    }
    return is;
  }
  friend ostream &operator<<(ostream &is, BigInt a) {
    for (; a.len > 1 && !a[a.len - 1]; a.len--)
      ;
    if (a.pm == -1) {
      cout << '-';
    }
    for (int i = a.len - 1; i >= 0; i--) {
      is << binary_symbol[a[i]];
    }
    return is;
  }
  T1 &operator[](int i) { return num[i]; }
  void operator=(const BigInt &b) {
    len = b.len;
    for (int i = 0; i < len; i++) {
      num[i] = b.num[i];
    }
  }
  bool operator<(const BigInt &b) {
    if (len != b.len) {
      return len < b.len;
    }
    for (int i = len - 1; i >= 0; i--) {
      if (num[i] != b.num[i]) {
        return num[i] < b.num[i];
      }
    }
    return 0;
  }
  void operator++() {
    num[0]++;
    for (int i = 0; i < len - 1; i++) {
      num[i + 1] += num[i] / binary;
      num[i] %= binary;
    }
  }
  void operator--() {
    num[0]--;
    for (int i = 0; i < len - 1; i++) {
      if (num[i] < 0) {
        num[i] += binary, num[i + 1]--;
      }
    }
  }
  BigInt operator+(const BigInt &b) {
    BigInt<Maxlen> c;
    c.len = max(len, b.len) + 1;
    for (int i = 0; i < c.len; i++) {
      c[i] = num[i] + b.num[i];
    }
    for (int i = 0; i < c.len - 1; i++) {
      c[i + 1] += c[i] / binary;
      c[i] %= binary;
    }
    return c;
  }
  BigInt operator-(const BigInt &b) {
    if (*this < b) {
      cout << '-';
      BigInt tmp = b;
      return tmp - *this;
    }
    BigInt<Maxlen> c;
    c.len = len;
    for (int i = 0; i < c.len; i++) {
      c[i] = num[i] - b.num[i];
    }
    for (int i = 0; i < c.len - 1; i++) {
      if (c[i] < 0) {
        c[i] += binary, c[i + 1]--;
      }
    }
    return c;
  }
  BigInt operator*(const T1 &b) {
    BigInt<Maxlen> c;
    c.len = len + 19;
    for (int i = 0; i < len; i++) {
      c[i] += num[i] * b;
    }
    for (int i = 0; i < c.len - 1; i++) {
      c[i + 1] += c[i] / binary;
      c[i] %= binary;
    }
    return c;
  }
  BigInt operator*(const BigInt &b) {
    BigInt<Maxlen> c;
    c.pm = (pm == b.pm ? 1 : -1);
    c.len = len + b.len + 1;
    for (int i = 0; i < len; i++) {
      for (int j = 0; j < b.len; j++) {
        c[i + j] += num[i] * b.num[j];
      }
    }
    for (int i = 0; i < c.len - 1; i++) {
      c[i + 1] += c[i] / binary;
      c[i] %= binary;
    }
    return c;
  }
  BigInt operator/(const T1 &b) {
    BigInt<Maxlen, T1> c;
    T1 r = 0;
    c.len = len;
    for (int i = len - 1; i >= 0; i--) {
      r = r * binary + num[i];
      c[i] = r / b;
      r %= b;
    }
    return c;
  }
  T1 operator%(const T1 &b) {
    T1 r = 0;
    for (int i = len - 1; i >= 0; i--) {
      r = r * binary + num[i];
      r %= b;
    }
    return r;
  }   
};

template <size_t len> void Input(BigInt<len> &a) {
  string s;
  cin >> s;
  a.len = s.size();
  for (int i = 0; i < a.len; i++) {
    if (s[a.len - i - 1] >= 'A' && s[a.len - i - 1] <= 'Z') {
      a[i] = s[a.len - i - 1] - 'A' + 10;
    } else {
      a[i] = s[a.len - i - 1] - '0';
    }
  }
}

template <size_t len> void Output(BigInt<len> &a) {
  for (; a.len > 1 && !a[a.len - 1]; a.len--)
    ;
  for (int i = a.len - 1; i >= 0; i--) {
    cout << binary_symbol[a[i]];
  }
}
}; // namespace Int

using namespace Int;
```
