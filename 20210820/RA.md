# C - Ra (Code mẫu ở dưới)
### Nguồn: [CodeChef - LIGHT](https://www.codechef.com/problems/LIGHT)

## Tóm tắt đề bài
- Có n điểm trên mặt phẳng _Oxy_, với hoành độ là `X[i]`, tung độ `Y[i]`.
- Từ mỗi điểm này, kẻ một tam giác cân, với góc tạo bởi đường cao và một trong 2 cạnh là `Z[i]` (độ).
- Kẻ một đoạn thẳng trên đường thẳng _y = M_, với hoành độ điểm bắt đầu là `L` và hoành độ điểm kết thúc là `R`. Tìm giá trị `M` lớn nhất, sao cho toàn bộ đoạn thẳng được kẻ nằm trong 1 hình tam giác nào đó.

## Ý tưởng  

- Gọi _độ che phủ_ của một điểm, tại một độ cao `m`, là giao của 2 cạnh bên của hình tam giác tạo được, với đường thẳng _y = m_.  

- Nhận xét dùng toán cơ bản, là với độ cao càng thấp, thì độ che phủ càng cao.  

=> Ta có thể giải bài toán này sử dụng Binary Search.

- Ta sẽ Binary Search giá trị M, rồi với mọi M ta tính _độ che phủ_ của mọi điểm, tại độ cao M.

- Sau đó, nếu các đoạn che phủ do mọi đèn, gộp các đoạn giao nhau lại, tạo được một đoạn thẳng che phủ cả đoạn từ L, đến R, thì ta kết luận là đúng.

- Quay lại việc tính độ che phủ: Hầu như các bạn ở đây đã học toán lớp 9, nên mọi người sẽ biết được rằng độ che phủ chính là: `(Y[i] - m) * tan(Z[i])`

## Một vài lời bình luận về cài đặt:

- Binary Search trên khoảng giá trị số thực yêu cần đặt một giá trị `eps`, nếu không vòng lặp sẽ diễn ra vô tận. Tương tự đối với mọi phép toán so sánh số thực.

- Do độ chính xác yêu cầu khá cao, trong khi `N` khá nhỏ `(N <= 50000)`, nên `eps` ta hoàn toàn có thể đặt nhỏ. (Đặt thậm chí lên đến `1e-13` vẫn có thể AC, và chú ý dùng kiểu `long double`.

- Server các OJ ngày nay [hy vọng là] chấm 64-bit nên không sao, nhưng với các trình chấm cổ lỗ sĩ như Themis, `long double` sẽ gặp một số vấn đề không hay. Để khắc phục điều này, ta có thể thêm 1 dòng code đơn giản:

```C++
	#pragma GCC target("fpmath=sse,sse2")
```
Chi tiết về dòng lệnh này ta có thể tham khảo trên [Codeforces](https://codeforces.com/blog/entry/78161)

- Nhớ kiểm tra nếu các đoạn gộp lại đủ đoạn `L`, `R` thì trả về đúng luôn, nhỡ sau này đủ đoạn `L`, `R` rồi mà ngắt ở đoạn khác rồi trả về `false` mất 4 sub ngồi debug 🐧🐧🐧

- Các hàm lượng giác của C++ sử dụng _radian_ thay vì độ. Nhớ chuyển đổi đơn vị!

## Link của nhà tài trợ
- [Ứng dụng cập nhật thời khóa biểu & vào lớp trong 1 nút bấm](http://azureams.github.io/TimetableApp.Uno).

- [Vừa nghe nhạc hay vừa ngắm cảnh đẹp](https://www.youtube.com/watch?v=HhjHYkPQ8F0). (Một lần nữa đây không phải Rickroll).  

- [Cần tuyển thành viên thông thạo XAML và C#](https://discord.gg/FvTNGn9YgA).

- Chương trình sinh test bằng XML hỗ trợ Themis (Coming soon).

## Code mẫu:
Và như đã hứa, đây là code mẫu 
```C++
#pragma GCC optimize ("Ofast")
#pragma GCC target("fpmath=sse,sse2")
#pragma GCC target("sse,sse2,sse3,ssse3,sse4,popcnt,abm,mmx,avx,avx2")
#ifdef AduMaster
#define _GLIBCXX_DEBUG
#define _GLIBCXX_DEBUG_PEDANTIC
#pragma GCC optimize "trapv"
#endif
using llong = long long;
using ullong = unsigned long long;
using ulong = unsigned long;
#include <bits/stdc++.h>
#ifdef AduMaster
#define DEBUG1(key, x) cerr << #key": " << (x) << endl;
#define DEBUG(x) cerr << #x": " << (x) << endl;
#define DEBUG_BOOLEAN(x) cerr << #x": " << boolalpha << bool(x) << noboolalpha << endl;
#define DEBUG_CONTAINER(x) {cerr << #x":" << endl; for (const auto & xx : (x)) cerr << xx << " "; cerr << endl;}
#define REPORT cerr << "Ok here" << endl;
#else
#define endl '\n'
#define DEBUG(x)
#define DEBUG1(key, x)
#define DEBUG_BOOLEAN(x)
#define DEBUG_CONTAINER(x)
#define REPORT
#endif
using namespace std;
#define PROBLEMNAME ""
//ifstream fin(PROBLEMNAME".INP");
//ofstream fout(PROBLEMNAME".OUT");
#ifdef AduMaster
//#define cin fin
//#define fout cout
#endif
//#pragma GCC poison cout
//#pragma GCC poison cin

#define var auto
#define refvar auto &
#define foreach for
#define in :

template <typename T1, typename T2>
inline bool minimize(T1 & t1, const T2 & t2)
{
	return (t1 > t2) ? (t1 = t2), true : false;
}

template <typename T1, typename T2>
inline bool maximize(T1 & t1, const T2 & t2)
{
	return (t1 < t2) ? (t1 = t2), true : false;
}

const long double eps = 1e-10;
const long double PI = acos(-1.0l);
const long double DegToRad = PI / 180;

struct Light	
{
	long double X;
	long double Y;
	long double Z;

	long double GroundLength;

	pair<long double, long double> GetCover(const long double & h) const
	{
		if (h - Y > eps)
		{
			// LMAO
			return {-1024, -1024};
		}
		// Thales?
		long double currentLength = (1.0 - (h / Y)) * GroundLength;
		return {X - currentLength, X + currentLength};
	}
};

bool Check(const vector<Light> & lights, const long double & L, const long double & R, const long double & height)
{
	vector<pair<long double, long double>> cover(lights.size());
	transform(lights.begin(), lights.end(), cover.begin(), [&](const Light & l)
	{
		return l.GetCover(height);
	});
	sort(cover.begin(), cover.end());

	pair<long double, long double> currentCover = cover[0];

	if ((currentCover.first - L <= eps) && (currentCover.second - R >= eps))
	{
		return true;
	}

	for (int i = 1; i < cover.size(); ++i)
	{
		if (cover[i].first - currentCover.second <= eps)
		{
			maximize(currentCover.second, cover[i].second);
			// Some stuff might interrupt mid-loop, we must
			// return as soon as possible.
			if ((currentCover.first - L <= eps) && (currentCover.second - R >= eps))
			{
				return true;
			}
		}
		else
		{
			// No trouble yet, the last ranges are shit.
			if (cover[i].first - L < eps)
			{
				currentCover = cover[i];
			}
			else
			{
				return false;
			}
		}
	}

	return (currentCover.first - L <= eps) && (currentCover.second - R >= eps);
}

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(nullptr);

	int N;
	long double L, R;
	cin >> N >> L >> R;

	vector<Light> lights(N);
	for (auto & l : lights)
	{
		cin >> l.X >> l.Y >> l.Z;

		// tan is a costly operation, so we should f this off
		// once and for all.
		l.GroundLength = tan(l.Z * DegToRad) * l.Y;
	}

	long double left = eps;
	long double right = 1000;

	while (left - right <= eps)
	{
		long double mid = (left + right) / 2;
		
		if (Check(lights, L, R, mid))
		{
			left = mid + eps;
		}
		else
		{
			right = mid - eps;
		}
	}

	cout << fixed << setprecision(9) << right;

	// Reminders from Ishar, from the Carano School of Magic:
	// Mistyped some variable name?
	// Forgot to cast some int to long long?
	// Copy-pasted some code blocks, but forgot to copy the patch?
	// You are not alone.
	return 0;
}
```

Code mẫu này được tài trợ bởi Học Viện Carano

