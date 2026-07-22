# SoC Endterm Report

Here are my IDs for different websites from which I have practiced CP questions:
1. Codeforces: aagam22
2. leetcode: aagam22
3. CSES: aagam22

I have studied theory from chapters 1-20 of the [CP Handbook](https://cses.fi/book/book.pdf) provided.

## Problems and Solutions

Below is the list of topic wise problems I have solved along with my solution code to each of them:

### Sorting & Searching
- [Distinct Numbers](https://cses.fi/problemset/task/1621)
```  
#include <iostream>
#include <algorithm>
using namespace std;
 
int main(){
    int n;
    cin >> n;
 
    int arr[n];
    for (int i=0;i<n;i++){
        cin >> arr[i];
    }
 
    sort(arr, arr+n);
 
    int num=1;
    for (int i=1;i<n;i++){
        if (arr[i] != arr[i-1]) num++;
    }
    cout << num;
}
```

- [Apartments](https://cses.fi/problemset/task/1084)
```
#include <iostream>
#include <algorithm>
using namespace std;
 
int main(){
    int n, m, k;
    cin >> n >> m >> k;
 
    int applicant[n];
    for(int i = 0; i < n; i++){
        cin >> applicant[i];
    }
 
    int apt[m];
    for(int i = 0; i < m; i++){
        cin >> apt[i];
    }
 
    sort(applicant, applicant + n);
    sort(apt, apt + m);
 
    int count = 0;
    int look = 0;
 
    for (int i=0; i<n; i++){
        while (look<m && apt[look] < applicant[i]-k) look++;
        if (look < m && apt[look]<=applicant[i] + k){
            count++;
            look++;
        }
    }
    cout << count << endl;
}
```
- [Ferris Wheel](https://cses.fi/problemset/task/1090)
```
#include <iostream>
#include <algorithm>
using namespace std;
 
int main(){
    int n, x;
    cin >> n >> x;
 
    int weights[n];
    for(int i = 0; i < n; i++){
        cin >> weights[i];
    }
 
    sort(weights, weights + n);
 
    int l = 0;
    int h = n-1;
    int num = 0;
 
    while (l <= h){
        if (weights[h] == x){
            num++;
            h--;
            continue;
        }
 
        if (weights[h] < x/2) break;
 
        if (weights[h] >= x/2){
            if (weights[l] + weights[h] <= x){
                num++;
                l++;
                h--;
            } else {
                num++;
                h--;
            }
        }
    }
    cout << num + (n - (l + (n-1-h)) + 1) / 2;
}
```
- [Sort Colors](https://leetcode.com/problems/sort-colors/)
```
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int zeros = 0;
        int ones = 1;
        int twos = 2;

        for (int i = 0; i < nums.size(); i++){
            if (nums[i] == 0) zeros++;
            else if (nums[i] == 1) ones++;
            else twos++;
        }

        for (int i = 0; i < nums.size(); i++){
            if (i < zeros) nums[i] = 0;
            else if (i < zeros + ones - 1) nums[i] = 1;
            else nums[i] = 2;
        }
    }
};
```

### Binary Search
- [Binary Search](https://leetcode.com/problems/binary-search/)
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int low = 0;
        int high = n - 1;
        int mid;

        while(high > low){
            mid = (low + high) / 2;
            if (nums[mid] == target) return mid;
            else if (nums[mid] > target) high = mid;
            else low = mid + 1;
        }

        if (nums[low] == target) return low;
        else return -1;
    }
};
```
- [Factory Machines](https://cses.fi/problemset/task/1620)
```
#include <iostream>
#include <algorithm>
using namespace std;
 
int main(){
    long long n, t;
    cin >> n >> t;
 
    long long times[n];
    for(long long i = 0; i < n; i++){
        cin >> times[i];
    }
 
    sort(times, times + n);
 
    long long time = 0;
    long long low = times[0];
    long long high = t*times[0];
    long long mid;
 
    while (low < high){
        mid = (low + high) / 2;
        long long count = 0;
 
        for(long long i = 0; i < n; i++){
            count += mid / times[i];
        }
 
        if (count >= t){
            time = mid;
            high = mid;
        } else {
            low = mid + 1;
        }
    }
 
    long long count = 0;
    for(long long i = 0; i < n; i++){
        count += low / times[i];
    }
 
    if (count >= t){
        time = low;
    }
 
    cout << time;
}
```

### Two Pointers & Sliding Window
- [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
```
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size();
        int l = 0;
        int r = n-1;
        int max = 0;

        while (l < r){
            int area = min(height[l], height[r]) * (r - l);
            if (area >= max) max = area;

            if (height[l] < height[r]) l++;
            else r--;
        }

        return max;
    }
};
```
- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        vector<int> last(256, -1);
        int ans = 0;
        int l = 0;

        for (int r = 0; r < s.size(); r++) {
            if (last[s[r]] >= l) {
                l = last[s[r]] + 1;
            }

            last[s[r]] = r;
            ans = max(ans, r - l + 1);
        }

        return ans;
    }
};
```

### Prefix Sums
- [Static Range Sum Queries](https://cses.fi/problemset/task/1646)
```
#include <iostream>
#include <algorithm>
using namespace std;
 
int main(){
    long long n, q;
    cin >> n >> q;
 
    long long nums[n];
    for(long long i = 0; i < n; i++){
        cin >> nums[i];
    }
 
    long long tests[2*q];
    for (long long i=0; i<q; i++){
        cin >> tests[2*i] >> tests[2*i+1];
    }
 
    long long sums[n];
    sums[0] = nums[0];
    for (long long i=1; i<n; i++){
        sums[i] = sums[i-1] + nums[i];
    }
 
    for (long long i=0; i<q; i++){
        long long sum;
        if (tests[2*i] == 1){
            sum = sums[tests[2*i+1]-1];
        } 
        else {
            sum = sums[tests[2*i+1]-1] - sums[tests[2*i]-2];
        }
 
        cout << sum << endl;
    }
}
```
- [Forest Queries](https://cses.fi/problemset/task/1652)
```
#include <iostream>
#include <algorithm>
using namespace std;
 
int main(){
    int n, q;
    cin >> n >> q;
 
    int forest[n][n];
    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            char x;
            cin >> x;
 
            if (x == '*') forest[i][j] = 1;
            else forest[i][j] = 0;
        }
    }
 
    int nums[n][n];
    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            nums[i][j] = forest[i][j];
            if (i > 0) nums[i][j] += nums[i-1][j];
            if (j > 0) nums[i][j] += nums[i][j-1];
            if (i > 0 && j > 0) nums[i][j] -= nums[i-1][j-1];
        }
    }
 
    for(int i = 0; i < q; i++){
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        x1--;
        y1--;
        x2--;
        y2--;
 
        int sum = nums[x2][y2];
        if (x1 > 0) sum -= nums[x1-1][y2];
        if (y1 > 0) sum -= nums[x2][y1-1];
        if (x1 > 0 && y1 > 0) sum += nums[x1-1][y1-1];
 
        cout << sum << endl;
    }
}
```

### Greedy (CSES)
- [Movie Festival](https://cses.fi/problemset/task/1629)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
using namespace std;
 
int main(){
    int n;
    cin >> n;    
 
    pair<int, int> times[n];
    for (int i=0; i<n; i++){
        cin >> times[i].second >> times[i].first;
    }
 
    sort(times, times+n);
 
    int num = 1;
    int last_end = times[0].first;
    for (int i=1; i<n; i++){
        if (times[i].second >= last_end){
            num++;
            last_end = times[i].first;
        }
    }
 
    cout << num;
}
```
- [Tasks and Deadlines](https://cses.fi/problemset/task/1630)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
using namespace std;
 
int main(){
    long long n;
    cin >> n;    
 
    pair<long long, long long> times[n];
    for (long long i=0; i<n; i++){
        cin >> times[i].first >> times[i].second;
    }
 
    sort(times, times+n);
 
    long long time = 0;
    long long reward = 0;
    for (long long i=0; i<n; i++){
        time += times[i].first;
        reward += times[i].second - time;
    }
 
    cout << reward;
}
```
- [Stick Lengths](https://cses.fi/problemset/task/1074)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
using namespace std;
 
int main(){
    long long n;
    cin >> n;    
 
    long long lengths[n];
    for (int i=0; i<n; i++){
        cin >> lengths[i];
    }
 
    sort(lengths, lengths+n);
 
    long long final = lengths[n/2];
    long long cost = 0;
 
    for (int i=0; i<n; i++){
        cost += abs(lengths[i] - final);
    }
 
    cout << cost;
}
```
- [Movie Festival II](https://cses.fi/problemset/task/1632)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
using namespace std;
 
int main(){
    long long n, k;
    cin >> n >> k;    
 
    pair<long long, long long> times[n];
    for (int i=0; i<n; i++){
        cin >> times[i].second >> times[i].first;
    }
 
    sort(times, times+n);
 
    multiset<long long> endings;
 
    for (int i=0; i<k; i++) {
        endings.insert(0);
    }
 
    long long ans = 0;
 
    for (int i=0; i<n; i++) {
        auto it = endings.upper_bound(times[i].second);
 
        if (it != endings.begin()) {
            it--;
 
            endings.erase(it);
            endings.insert(times[i].first);
 
            ans++;
        }
    }
 
    cout << ans;
}
```
- [Reading Books](https://cses.fi/problemset/task/1631)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
using namespace std;
 
int main(){
    long long n;
    cin >> n;
 
    long long time[n];
    for (int i=0; i<n; i++){
        cin >> time[i];
    }
 
    sort(time, time+n);
 
    long long time1=0, time2=2*time[n-1];
 
    for (int i=0; i<n; i++){
        time1 += time[i];
    }
 
    cout << max(time1, time2);
}
```
### DP
- [Dice Combinations](https://cses.fi/problemset/task/1633)
```
#include <iostream>
using namespace std;

int main(){
    int n;
    cin >> n;

    const int mod = 1000000007;

    long long dp[n+1];
    dp[0] = 1;

    for(int i=1; i<=n; i++){
        dp[i] = 0;
        for(int j=1; j<=6; j++){
            if(i-j >= 0){
                dp[i] += dp[i-j];
                dp[i] %= mod;
            }
        }
    }

    cout << dp[n];
}
```
- [Minimizing Coins](https://cses.fi/problemset/task/1634)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int n, x;
    cin >> n >> x;

    int coin[n];
    for(int i=0; i<n; i++){
        cin >> coin[i];
    }

    int dp[x+1];

    dp[0] = 0;
    for(int i=1; i<=x; i++){
        dp[i] = 1000000000;
    }

    for(int i=1; i<=x; i++){
        for(int j=0; j<n; j++){
            if(i >= coin[j]){
                dp[i] = min(dp[i], dp[i-coin[j]]+1);
            }
        }
    }

    if(dp[x] == 1000000000){
        cout << -1;
    }
    else{
        cout << dp[x];
    }
}
```
- [Coin Combinations I](https://cses.fi/problemset/task/1635)
```
#include <iostream>
using namespace std;

int main(){
    int n, x;
    cin >> n >> x;

    int coin[n];
    for(int i=0; i<n; i++){
        cin >> coin[i];
    }

    const int mod = 1000000007;

    long long dp[x+1];
    dp[0] = 1;

    for(int i=1; i<=x; i++){
        dp[i] = 0;
        for(int j=0; j<n; j++){
            if(i >= coin[j]){
                dp[i] += dp[i-coin[j]];
                dp[i] %= mod;
            }
        }
    }

    cout << dp[x];
}
```
- [Coin Combinations II](https://cses.fi/problemset/task/1636)
```
#include <iostream>
using namespace std;

int main(){
    int n, x;
    cin >> n >> x;

    int coin[n];
    for(int i=0; i<n; i++){
        cin >> coin[i];
    }

    const int mod = 1000000007;

    int dp[x+1];

    dp[0] = 1;
    for(int i=1; i<=x; i++){
        dp[i] = 0;
    }

    for(int i=0; i<n; i++){
        for(int j=coin[i]; j<=x; j++){
            dp[j] += dp[j-coin[i]];
            dp[j] %= mod;
        }
    }

    cout << dp[x];
}
```
- [Removing Digits](https://cses.fi/problemset/task/1637)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int n;
    cin >> n;

    int dp[n+1];

    dp[0] = 0;

    for(int i=1; i<=n; i++){
        dp[i] = 1000000000;

        int x = i;
        while(x){
            int d = x%10;
            if(d){
                dp[i] = min(dp[i], dp[i-d]+1);
            }
            x /= 10;
        }
    }

    cout << dp[n];
}
```
- [Grid Paths](https://cses.fi/problemset/task/1638)
```
#include <iostream>
using namespace std;

int main(){
    int n;
    cin >> n;

    const int mod = 1000000007;

    char grid[n][n];
    for(int i=0; i<n; i++){
        for(int j=0; j<n; j++){
            cin >> grid[i][j];
        }
    }

    int dp[n][n];

    for(int i=0; i<n; i++){
        for(int j=0; j<n; j++){
            dp[i][j] = 0;
        }
    }

    if(grid[0][0] == '.'){
        dp[0][0] = 1;
    }

    for(int i=0; i<n; i++){
        for(int j=0; j<n; j++){
            if(grid[i][j] == '*'){
                continue;
            }

            if(i){
                dp[i][j] += dp[i-1][j];
                dp[i][j] %= mod;
            }

            if(j){
                dp[i][j] += dp[i][j-1];
                dp[i][j] %= mod;
            }
        }
    }

    cout << dp[n-1][n-1];
}
```
- [Book Shop](https://cses.fi/problemset/task/1158)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int n, x;
    cin >> n >> x;

    int price[n], pages[n];

    for(int i=0; i<n; i++){
        cin >> price[i];
    }

    for(int i=0; i<n; i++){
        cin >> pages[i];
    }

    int dp[x+1];

    for(int i=0; i<=x; i++){
        dp[i] = 0;
    }

    for(int i=0; i<n; i++){
        for(int j=x; j>=price[i]; j--){
            dp[j] = max(dp[j], dp[j-price[i]]+pages[i]);
        }
    }

    cout << dp[x];
}
```
- [Edit Distance](https://cses.fi/problemset/task/1639)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    string a, b;
    cin >> a >> b;

    int n = a.size();
    int m = b.size();

    int dp[n+1][m+1];

    for(int i=0; i<=n; i++){
        dp[i][0] = i;
    }

    for(int j=0; j<=m; j++){
        dp[0][j] = j;
    }

    for(int i=1; i<=n; i++){
        for(int j=1; j<=m; j++){
            dp[i][j] = min(dp[i-1][j]+1, dp[i][j-1]+1);
            dp[i][j] = min(dp[i][j], dp[i-1][j-1]+(a[i-1]!=b[j-1]));
        }
    }

    cout << dp[n][m];
}
```
- [Increasing Subsequence](https://cses.fi/problemset/task/1145)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int n;
    cin >> n;

    int a[n];
    int dp[n];
    int len = 0;

    for(int i=0; i<n; i++){
        cin >> a[i];

        int pos = lower_bound(dp, dp+len, a[i]) - dp;
        dp[pos] = a[i];

        if(pos == len){
            len++;
        }
    }

    cout << len;
}
```
- [Projects](https://cses.fi/problemset/task/1140)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int n;
    cin >> n;

    long long a[n], b[n], p[n];

    for(int i=0; i<n; i++){
        cin >> a[i] >> b[i] >> p[i];
    }

    int ind[n];
    for(int i=0; i<n; i++){
        ind[i] = i;
    }

    sort(ind, ind+n, [&](int x, int y){
        return b[x] < b[y];
    });

    long long dp[n+1];
    dp[0] = 0;

    for(int i=1; i<=n; i++){
        int l = 0, r = i-2;
        int pos = -1;

        while(l <= r){
            int mid = (l+r)/2;
            if(b[ind[mid]] < a[ind[i-1]]){
                pos = mid;
                l = mid+1;
            }
            else{
                r = mid-1;
            }
        }

        dp[i] = max(dp[i-1], p[ind[i-1]] + dp[pos+1]);
    }

    cout << dp[n];
}
```


### Codeforces
- [Interesting Drink](https://codeforces.com/problemset/problem/706/B)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int n;
    cin >> n;
    int x[n];
    for(int i = 0; i < n; i++){
        cin >> x[i];
    }
    sort(x, x + n);

    int q;
    cin >> q;
    int m[q];
    for(int i = 0; i < q; i++){
        cin >> m[i];

        int l = 0;
        int h = n-1;
        while(l<h){
            int mid = (l+h)/2;
            if (x[mid+1] > m[i]){
                h = mid;
            }
            else{
                l = mid+1;
            }
        }

        if (l==0 && x[l] > m[i]) cout << 0 << endl;
        else cout << l+1 << endl;
    }
}
```
- [Books](https://codeforces.com/problemset/problem/279/B)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int n, t;
    cin >> n >> t;

    int a[n];
    for(int i = 0; i < n; i++){
        cin >> a[i];
    }

    int max = 0;
    int sum = a[0];
    int l = 0;
    int r = 0;

    while (r<n){
        if (sum <= t){
            if (r-l+1 > max) max = r-l+1;
            sum += a[r+1];
            r++;
        }
        else{
            sum -= a[l];
            l++;
        }
    }
    cout << max << endl;
}
```
- [Number of Pairs](https://codeforces.com/problemset/problem/1538/C)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;

    for (int i=0; i<t; i++){
        int n, l, r;
        cin >> n >> l >> r;

        int a[n];
        for (int j=0; j<n; j++){
            cin >> a[j];
        }

        long long countl = 0;
        long long countr = 0;
        sort(a, a+n);

        int left=0;
        int right = n-1;
        while (left < right){
            if (a[left] + a[right] <= r){
                countr += right - left;
                left++;
            }
            else right--;
        }

        left = 0;
        right = n-1;
        while (left < right){
            if (a[left] + a[right] < l){
                countl += right - left;
                left++;
            }
            else right--;
        }
        cout << countr - countl << endl;
    }
}
```
- [Ilya and Queries](https://codeforces.com/problemset/problem/313/B)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    string s;
    cin >> s;
    int n = s.length();

    int same[n-1];
    for(int i = 0; i < n-1; i++){
        if (s[i] == s[i+1]) same[i] = 1;
        else same[i] = 0;
    }

    int sums[n];
    sums[0] = 0;
    for(int i = 1; i < n; i++){
        sums[i] = sums[i-1] + same[i-1];
    }

    int m;
    cin >> m;
    for (int i=0; i<m; i++){
        int l, r;
        cin>>l>>r;
        cout << sums[r-1] - sums[l-1] << endl;
    }
}
```
- [Little Girl and Maximum Sum](https://codeforces.com/problemset/problem/276/C)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    long long n, q;
    cin >> n >> q;

    long long a[n];
    for(long long i = 0; i < n; i++){
        cin >> a[i];
    }

    long long diff[n+1];
    fill(diff, diff + n + 1, 0);
    for (long long i=0; i<q; i++){
        long long l, r;
        cin >> l >> r;
        l--;
        r--;
        diff[l]++;
        if (r+1<n) diff[r+1]--;
    }
    
    long long freq[n];
    freq[0] = diff[0];
    for (long long i=1; i<n; i++){
        freq[i] = freq[i-1] + diff[i];
    }

    sort(a, a+n);
    sort(freq, freq+n);

    long long ans = 0;
    for (long long i=0; i<n; i++){
        ans += a[i] * freq[i];
    }
    cout << ans;
}
```
- [Sponsor of Your Problems](https://codeforces.com/problemset/problem/2121/E)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;

    while(t--){
        string l, r;
        cin >> l >> r;

        int n = l.size();

        const int inf = 1000000000;

        int dp[11][2][2];

        for(int i=0; i<=n; i++){
            for(int j=0; j<2; j++){
                for(int k=0; k<2; k++){
                    dp[i][j][k] = inf;
                }
            }
        }

        dp[0][1][1] = 0;

        for(int i=0; i<n; i++){
            for(int a=0; a<2; a++){
                for(int b=0; b<2; b++){
                    if(dp[i][a][b] == inf){
                        continue;
                    }

                    int lo = a ? l[i]-'0' : 0;
                    int hi = b ? r[i]-'0' : 9;

                    for(int d=lo; d<=hi; d++){
                        int na = a && (d==lo);
                        int nb = b && (d==hi);

                        dp[i+1][na][nb] = min(dp[i+1][na][nb],
                                             dp[i][a][b] + (d==l[i]-'0') + (d==r[i]-'0'));
                    }
                }
            }
        }

        int ans = inf;
        for(int i=0; i<2; i++){
            for(int j=0; j<2; j++){
                ans = min(ans, dp[n][i][j]);
            }
        }

        cout << ans << "\n";
    }
}
```
- [Gellyfish and Flaming Peony](https://codeforces.com/problemset/problem/2115/A)
```
#include <iostream>
#include <algorithm>
using namespace std;

int gcd(int a, int b){
    while(b){
        int t = a%b;
        a = b;
        b = t;
    }
    return a;
}

int main(){
    int t;
    cin >> t;

    while(t--){
        int n;
        cin >> n;

        int a[n];
        int g = 0;

        for(int i=0; i<n; i++){
            cin >> a[i];
            if(i == 0){
                g = a[i];
            }
            else{
                g = gcd(g, a[i]);
            }
        }

        int cnt = 0;
        for(int i=0; i<n; i++){
            if(a[i] == g){
                cnt++;
            }
        }

        if(cnt){
            cout << n-cnt << "\n";
            continue;
        }

        const int inf = 1000000000;

        int dp[5001];
        for(int i=0; i<=5000; i++){
            dp[i] = inf;
        }

        for(int i=0; i<n; i++){
            int ndp[5001];
            for(int j=0; j<=5000; j++){
                ndp[j] = dp[j];
            }

            ndp[a[i]] = 1;

            for(int j=1; j<=5000; j++){
                if(dp[j] == inf){
                    continue;
                }
                int x = gcd(j, a[i]);
                ndp[x] = min(ndp[x], dp[j]+1);
            }

            for(int j=0; j<=5000; j++){
                dp[j] = ndp[j];
            }
        }

        cout << dp[g]-1+n-1 << "\n";
    }
}
```
- [Test of Love](https://codeforces.com/problemset/problem/1992/D)
```
#include <iostream>
using namespace std;

int main(){
    int t;
    cin >> t;

    while(t--){
        int n, m, k;
        cin >> n >> m >> k;

        string s;
        cin >> s;

        int dp[n+2];

        for(int i=0; i<=n+1; i++){
            dp[i] = -1;
        }

        dp[0] = k;

        for(int i=1; i<=n+1; i++){
            if(i != n+1 && s[i-1] == 'C'){
                continue;
            }

            for(int j=1; j<=m; j++){
                if(i-j >= 0 && (i-j == 0 || s[i-j-1] == 'L')){
                    dp[i] = max(dp[i], dp[i-j]);
                }
            }

            if(i > 1 && s[i-2] == 'W'){
                dp[i] = max(dp[i], dp[i-1]-1);
            }
        }

        if(dp[n+1] >= 0){
            cout << "YES\n";
        }
        else{
            cout << "NO\n";
        }
    }
}
```
- [Magnitude (Easy Version)](https://codeforces.com/problemset/problem/1984/C1)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;

    while(t--){
        int n;
        cin >> n;

        long long mn = 0, mx = 0;

        for(int i=0; i<n; i++){
            long long x;
            cin >> x;

            long long a = mn + x;
            long long b = mx + x;

            mn = min(a, abs(b));
            mx = max(abs(a), abs(b));
        }

        cout << mx << "\n";
    }
}
```

### Themed Contests
- [Pens and Pencils](https://codeforces.com/group/hoDVdIvtE3/contest/695864/problem/A)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;
    for (int i=0; i<t; i++){
        int a, b, c, d, k;
        cin >> a >> b >> c >> d >> k;
        int x, y;
        if (a%c == 0) x = a/c;
        else x = a/c + 1;
        if (b%d == 0) y = b/d;
        else y = b/d + 1;

        if (x+y > k) cout << -1 << endl;
        else cout << x << " " << y << endl;
    }
}
```
- [Buying Torches](https://codeforces.com/group/hoDVdIvtE3/contest/695864/problem/B)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;
    for (int i=0; i<t; i++){
        long long x, y, k;
        cin >> x >> y >> k;

        long long total = k*(y+1) - 1;
        long long net = (total+x-2)/(x-1);
        cout << net + k << endl;
    }
}
```
- [Joyboard](https://codeforces.com/group/hoDVdIvtE3/contest/695864/problem/C)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;
    for (int i=0; i<t; i++){
        long long n, m, k;
        cin >> n >> m >> k;

        if (k == 1){
            cout << 1 << endl;
        }

        else if (k == 2){
            cout << m/n + min(m, n-1) << endl;
        }

        else if (k == 3){
            cout << m - m/n - min(m, n-1) << endl;
        }

        else cout << 0 << endl;
    }
}
```
- [Shawarma Tent](https://codeforces.com/group/hoDVdIvtE3/contest/695864/problem/D)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    long long n, sx, sy;
    cin >> n >> sx >> sy;

    long long t=0, b=0, l=0, r=0;
    for (int i=0; i<n; i++){
        long long x, y;
        cin >> x >> y;

        if (x > sx) r++;
        if (x < sx) l++;

        if (y > sy) t++;
        if (y < sy) b++;
    }

    long long m = max({t, b, l, r});
    cout << m << endl;

    if (m == t) cout << sx << " " << sy+1 << endl;
    else if (m == b) cout << sx << " " << sy-1 << endl;
    else if (m == l) cout << sx-1 << " " << sy << endl;
    else if (m == r) cout << sx+1 << " " << sy << endl;


}
```
- [Modulo Equality](https://codeforces.com/group/hoDVdIvtE3/contest/695864/problem/E)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    long long n, m;
    cin >> n >> m;
    long long a[n];
    for(long long i = 0; i < n; i++){
        cin >> a[i];
    }
    long long b[n];
    for(long long i = 0; i < n; i++){
        cin >> b[i];
    }

    sort(a, a + n);
    sort(b, b + n);

    long long ans = m;
    for (int i=0; i<n; i++){
        long long x = (b[0] - a[i] + m) % m;
        long long c[n];
        for (int j=0; j<n; j++){
            c[j] = (a[j] + x) % m;
        }

        sort(c, c + n);
        if (equal(c, c + n, b)){
            ans = min(ans, x);
        }
    }
    cout << ans << endl;
}
```
- [Guess the k-th Zero (Easy Version)](https://codeforces.com/group/hoDVdIvtE3/contest/695864/problem/F)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int n, t;
    cin >> n >> t;

    for (int i=0; i<t; i++){
        int k;
        cin >> k;

        int l=1, r=n;
        while (l<r){
            int mid = (l+r)/2;

            cout << "?" << " " << 1 << " " << mid << "\n";
            cout.flush();

            int sum;
            cin >> sum;
            int zeros = mid - sum;

            if (zeros >= k){
                r = mid;
            } 
            else {
                l = mid + 1;
            }
        }

        cout << "!" << " " << l << "\n";
        cout.flush();
    }
}
```
- [Polycarp and Sum of Subsequences](https://codeforces.com/group/hoDVdIvtE3/contest/695993/problem/A)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;

    while(t--){
        int n[7];
        for(int i = 0; i < 7; i++){
            cin >> n[i];
        }

        sort(n, n + 7);

        int a[3];
        a[0] = n[0];
        a[1] = n[1];
        a[2] = n[6] - n[0] - n[1];
        cout << a[0] << " " << a[1] << " " << a[2] << "\n";
    }
}
```
- [Sum of Two Numbers](https://codeforces.com/group/hoDVdIvtE3/contest/695993/problem/B)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;
    while(t--){
        int n;
        cin >> n;

        if (n%2 == 0){
            cout << n/2 << " " << n/2 << "\n";
        }
        else{
            int x=0, y=0;
            int power=1;
            int a=0;
            while (n > 0){
                int d = n%10;
                if (d%2 == 0){
                    x += (d/2)*power;
                    y += (d/2)*power;
                }
                else{
                    if (a%2 == 0){
                        x += (d/2 + 1)*power;
                        y += (d/2)*power;
                    }
                    else{
                        x += (d/2)*power;
                        y += (d/2 + 1)*power;
                    }
                    a++;
                }
                n /= 10;
                power *= 10;
            }
            cout << x << " " << y << "\n";
        }
    }
}
```
- [The Delivery Dilemma](https://codeforces.com/group/hoDVdIvtE3/contest/695993/problem/C)
```
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
    int t;
    cin >> t;
    while(t--){
        int n;
        cin >> n;
        int a[n];
        for(int i = 0; i < n; i++){
            cin >> a[i];
        }
        int b[n];
        for (int i=0; i<n; i++){
            cin >> b[i];
        }
        
        long long l = 0;
        long long r = 0;
        for (long long i=0; i<n; i++){
            r += b[i];
        }

        while (l<r){
            long long mid = (l+r)/2;
            long long net = 0;
            for (long long i=0; i<n; i++){
                if (a[i] <= mid) continue;
                else net += b[i];
            }
            if (net > mid) l = mid+1;
            else r = mid;
        }
        cout << l << endl;
    }
}
```
- [Meeting on the Line](https://codeforces.com/group/hoDVdIvtE3/contest/695993/problem/D)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
using namespace std;

int main(){
    int t;
    cin >> t;
    while(t--){
        int n;
        cin >> n;
        double x[n];
        for(int i = 0; i < n; i++){
            cin >> x[i];
        }
        double t[n];
        for(int i = 0; i < n; i++){
            cin >> t[i];
        }

        double l=0;
        double r=1e9;

        for(int i=0; i<100; i++){
            double mid = (l + r) / 2.0;
            double L = -1e9;
            double R = 1e9;

            bool can = true;
            for(int j=0; j<n; j++){
                if (mid < t[j]){
                    can = false;
                    break;
                }

                double rem = mid - t[j];

                L = max(L, x[j] - rem);
                R = min(R, x[j] + rem);
            }

            if (can && L <= R){
                r = mid;
            }
            else{
                l = mid;
            }
        }

        double x0 = -1e9;
        for(int i=0; i<n; i++){
            double rem = r - t[i];
            x0 = max(x0, x[i] - rem);
        }
        cout << fixed << setprecision(10) <<x0 << "\n";
    }
}
```
- [Petya and the Exam](https://codeforces.com/group/hoDVdIvtE3/contest/695993/problem/E)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
using namespace std;

int main(){
    int m;
    cin >> m;
    while(m--){
        long long n, T, a, b;
        cin >> n >> T >> a >> b;
        long long diff[n];
        for(long long i = 0; i < n; i++){
            cin >> diff[i];
        }
        long long t[n];
        for(long long i = 0; i < n; i++){
            cin >> t[i];
        }

        long long ans = 0;
        long long easy = 0;
        long long tough = 0;
        for (long long i=0; i<n; i++){
            if (diff[i] == 0) easy++;
            else tough++;
        }

        pair<long long, long long> arr[n];
        for (long long i=0; i<n; i++){
            arr[i] = {t[i], diff[i]};
        }
        sort(arr, arr+n);

        long long mandeasy = 0;
        long long mandtough = 0;
        long long i=0;
        while (i<n){
            long long leave;
            leave = arr[i].first - 1;

            long long req = mandeasy * a + mandtough * b;
            long long done = mandeasy + mandtough;
            long long easyleft = easy - mandeasy;
            long long toughleft = tough - mandtough;

            if (req <= leave){
                done += min(easyleft, (leave - req) / a);
                req += min(easyleft, (leave - req) / a) * a;
                done += min(toughleft, (leave - req) / b);
                req += min(toughleft, (leave - req) / b) * b;

                ans = max(ans, done);
            }


            long long newdeadline = arr[i].first;
            while (i<n && arr[i].first == newdeadline){
                if (arr[i].second == 0) mandeasy++;
                else mandtough++;
                i++;
            }
        }

        long long leave = T;

        long long req = mandeasy * a + mandtough * b;
        long long done = mandeasy + mandtough;
        long long easyleft = easy - mandeasy;
        long long toughleft = tough - mandtough;

        if (req <= leave){
            done += min(easyleft, (leave - req) / a);
            req += min(easyleft, (leave - req) / a) * a;
            done += min(toughleft, (leave - req) / b);
            req += min(toughleft, (leave - req) / b) * b;

            ans = max(ans, done);
        }

        cout << ans << "\n";
    }
}
```
- [Angry Monk](https://codeforces.com/group/hoDVdIvtE3/contest/697038/problem/A)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
using namespace std;

int main(){
    long long t;
    cin >> t;

    for (long long i=0; i<t; i++){
        long long n, k;
        cin >> n >> k;

        long long a[k];
        for (long long j=0; j<k; j++){
            cin >> a[j];
        }

        sort(a, a+k);

        long long steps = 0;
        for (long long j=0; j<k-1; j++){
            steps += a[j] - 1;
        }

        steps += n - a[k-1];
        cout << steps << "\n";
    }
}
```
- [Fruits](https://codeforces.com/group/hoDVdIvtE3/contest/697038/problem/B)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
using namespace std;

int main(){
    long long n, m;
    cin >> n >> m;

    long long prices[n];
    for (int i=0; i<n; i++) cin >> prices[i];

    sort(prices, prices+n);

    string fruits[n];
    long long nums[n];
    long long empty = 0;
    for (int i=0; i<n; i++) nums[i] = 0;

    for (int i=0; i<m; i++){
        string s;
        cin >> s;
        bool found = false;
        for (int j=0; j<empty; j++){
            if (s == fruits[j]){
                nums[j]++;
                found = true;
                break;
            }
        }

        if (found == false){
            fruits[empty] = s;
            nums[empty] = 1;
            empty++;
        }
    }

    sort(nums, nums+n);

    long long least=0, most=0;
    for (int i=0; i<n; i++){
        least += nums[i] * prices[i];
        most += nums[i] * prices[n-i-1];
    }

    cout << most << " " << least << "\n";
}
```
- [Equal Sums](https://codeforces.com/group/hoDVdIvtE3/contest/697038/problem/C)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
using namespace std;

int main(){
    long long k;
    cin >> k;

    unordered_map<long long, pair<int,int>> mp;

    for (int i = 1; i <= k; i++) {
        int n;
        cin >> n;

        vector<long long> a(n);
        long long sum = 0;

        for (int j = 0; j < n; j++) {
            cin >> a[j];
            sum += a[j];
        }

        for (int j = 0; j < n; j++) {
            long long val = sum - a[j];

            if (mp.count(val)) {
                int seq = mp[val].first;
                int pos = mp[val].second;

                if (seq != i) {
                    cout << "YES\n";
                    cout << seq << ' ' << pos << '\n';
                    cout << i << ' ' << (j + 1) << '\n';
                    return 0;
                }
            } else {
                mp[val] = {i, j + 1};
            }
        }
    }

    cout << "NO\n";
}
```
- [Potions (Hard Version)](https://codeforces.com/group/hoDVdIvtE3/contest/697554/problem/C)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

int main(){
    long long n;
    cin >> n;
    long long a[n];
    for (int i=0; i<n; i++) cin >> a[i];

    priority_queue<long long, vector<long long>, greater<long long>> pots;
    long long sum = 0;

    for (int i=0; i<n; i++){
        sum += a[i];
        pots.push(a[i]);

        if (sum < 0){
            sum -= pots.top();
            pots.pop();
        }
    }

    cout << pots.size() << "\n";
}
```
- [Just Eat It!](https://codeforces.com/group/hoDVdIvtE3/contest/697554/problem/G)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
using namespace std;

int main(){
    long long t;
    cin >> t;

    while(t--){
        long long n;
        cin >> n;

        long long a[n];
        for (int i=0; i<n; i++) cin >> a[i];

        long long sums[n];
        sums[0] = a[0];
        for (int i=1; i<n; i++) sums[i] = sums[i-1] + a[i];

        long long rev[n];
        rev[0] = a[n-1];
        for (int i=1; i<n; i++) rev[i] = rev[i-1] + a[n-i-1];

        bool found = false;
        for (int i=0; i<n; i++){
            if (sums[i] <= 0 || rev[i] <= 0){
                found = true;
                break;
            }
        }

        if (found == true) cout << "NO" << "\n";
        else cout << "YES" << "\n";
    }

}
```
- [Everyone Loves Tres](https://codeforces.com/group/hoDVdIvtE3/contest/698624/problem/A)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

int main(){
    long long t;
    cin >> t;

    while(t--){
        long long n;
        cin >> n;

        if (n%2 == 1){
            if (n<=3) cout << -1 << "\n";
            else{
                string ans;
                for (int i=0; i<n-4; i++) ans += '3';
                ans += '6';
                ans += '3';
                ans += '6';
                ans += '6';
                cout << ans << "\n";
            }
        }

        else{
            string ans;
            for (int i=0; i<n-2; i++) ans += '3';
            ans += '6';
            ans += '6';
            cout << ans << "\n";
        }
    }
}
```
- [Spotlights](https://codeforces.com/group/hoDVdIvtE3/contest/698624/problem/B)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

int main(){
    long long n, m;
    cin >> n >> m;

    long long pos[n][m];
    for (int i=0; i<n; i++){
        for (int j=0; j<m; j++) cin >> pos[i][j];
    }

    long long good = 0;

    long long left[n][m], right[n][m], up[n][m], down[n][m];

    for (int i=0; i<n; i++){
        bool seenl = false;
        for (int j=0; j<m; j++){
            left[i][j] = seenl;
            if (pos[i][j] == 1) seenl = true;
        }

        bool seenr = false;
        for (int j=m-1; j>=0; j--){
            right[i][j] = seenr;
            if (pos[i][j] == 1) seenr = true;
        }
    }

    for (int j=0; j<m; j++){
        bool seenl = false;
        for (int i=0; i<n; i++){
            up[i][j] = seenl;
            if (pos[i][j] == 1) seenl = true;
        }

        bool seenr = false;
        for (int i=n-1; i>=0; i--){
            down[i][j] = seenr;
            if (pos[i][j] == 1) seenr = true;
        }
    }

    for (int i=0; i<n; i++){
        for (int j=0; j<m; j++){
            if (pos[i][j] == 0) good += left[i][j] + right[i][j] + up[i][j] + down[i][j];
        }
    }

    cout << good;
}
```
- [Two Movies](https://codeforces.com/group/hoDVdIvtE3/contest/698624/problem/C)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

int main(){
    long long t;
    cin >> t;

    while(t--){
        long long n;
        cin >> n;

        int a[n], b[n];
        for (int i=0; i<n; i++) cin >> a[i];
        for (int i=0; i<n; i++) cin >> b[i];

        long long rev1 = 0, rev2 = 0, leftn = 0, leftp = 0;

        for (int i=0; i<n; i++){
            if (a[i] > b[i]) rev1 += a[i];
            else if (a[i] < b[i]) rev2 += b[i];
            else{
                if (a[i] == 1) leftp++;
                else if (a[i] == 0) continue;
                else leftn++;
            }
        }

        if (rev1 < rev2){
            long long temp = rev1;
            rev1 = rev2;
            rev2 = temp;
        }

        long long pos = min(leftp, rev1 - rev2);
        rev2 += pos;
        leftp -= pos;
        rev1 += (leftp + 1)/2;
        rev2 += leftp/2;

        long long neg = min(leftn, rev1-rev2);
        rev1 -= neg;
        leftn -= neg;

        cout << min(rev1, rev2) - (leftn+1)/2 << "\n";
    }
}
```
- [Food for Animals](https://codeforces.com/group/hoDVdIvtE3/contest/698781/problem/A)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

int main(){
    long long t;
    cin >> t;

    while(t--){
        long long a, b, c, x, y;
        cin >> a >> b >> c >> x >> y;

        x -= a;
        y -= b;
        if (x < 0) x = 0;
        if (y < 0) y = 0;
        long long ans = x + y - c;

        if (ans <= 0) cout << "YES" << "\n";
        else cout << "NO" << "\n";
    }
}
```
- [Di-visible Confusion](https://codeforces.com/group/hoDVdIvtE3/contest/698781/problem/B)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

int main(){
    long long t;
    cin >> t;

    while(t--){
        long long n;
        cin >> n;
        long long a[n];
        for (int i=0; i<n; i++) cin >> a[i];

        bool works = true;

        for (int i=0; i<n; i++){
            bool found = false;
            for (int j=1; j<=i+1; j++){
                if (a[i]%(j+1) != 0){
                    found = true;
                    break;
                }
            }

            if (found == false) works = false;
        }

        if (works == true) cout << "YES" << "\n";
        else cout << "NO" << "\n";
    }
}
```
- [Alice and the Cake](https://codeforces.com/group/hoDVdIvtE3/contest/698781/problem/C)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

int main(){
    long long t;
    cin >> t;

    while(t--){
        long long n;
        cin >> n;
        
        multiset<long long> a;
        long long total = 0;
        for (int i=0; i<n; i++){
            long long x;
            cin >> x;
            a.insert(x);
            total += x;
        }

        priority_queue<long long> pieces;
        pieces.push(total);

        bool works = true;

        while(a.empty() == false){
            if (pieces.empty() == true){
                works = false;
                break;
            }

            long long most = pieces.top();
            pieces.pop();

            if (a.find(most) != a.end()){
                a.erase(a.find(most));
                continue;
            }

            if (most == 1){
                works = false;
                break;
            }

            pieces.push(most/2);
            pieces.push((most+1)/2);

            if (pieces.size() > n){
                works = false;
                break;
            }

        }

        if (works == true) cout << "YES" << "\n";
        else cout << "NO" << "\n";
    }
}
```
- [Spy-string](https://codeforces.com/group/hoDVdIvtE3/contest/698781/problem/D)
```
#include <iostream>
#include <algorithm>
#include <iomanip>
#include <set>
#include <string>
#include <vector>
#include <unordered_map>
#include <queue>
using namespace std;

int main(){
    long long t;
    cin >> t;

    while(t--){
        long long n, m;
        cin >> n >> m;

        string a[n];
        for (int i=0; i<n; i++) cin >> a[i];

        string ans;

        for (int pos=0; pos<m; pos++){
            for (char c='a'; c<='z'; c++){
                string s = a[0];
                s[pos] = c;

                bool works = true;

                for (int i=0; i<n; i++){
                    int diff = 0;

                    for (int j=0; j<m; j++){
                        if (s[j] != a[i][j]) diff++;
                    }

                    if (diff > 1){
                        works = false;
                        break;
                    }
                }

                if (works == true){
                    ans = s;
                    break;
                }
            }

            if (ans.empty() == false) break;
        }

        if (ans.empty() == true) cout << -1 << "\n";
        else cout << ans << "\n";
    }
}
```
- [Iskander and Drawings](https://codeforces.com/contest/2244/problem/A)
```
#include <bits/stdc++.h>
using namespace std;

int main(){
    int t;
    cin >> t;
    while (t--){
        int n;
        cin >>n;
        string s;
        cin >> s;

        int maxi = 0;
        int len = 0;
        for (int i=0; i<n; i++){
            if ((i==0) && (s[i] == '#')) len = 1;
            else if (s[i] == '#') len++;
            else len = 0;

            maxi = max(maxi, len);
        }

        cout << (maxi+1)/2 << "\n";
    }
}
```
- [Nikita and Books](https://codeforces.com/contest/2244/problem/B)
```
#include <bits/stdc++.h>
using namespace std;

int main(){
    int t;
    cin >> t;
    while (t--){
        long long n;
        cin >> n;
        long long a[n];
        for (long long i=0; i<n; i++) cin >> a[i];

        long long sum[n];
        sum[0] = a[0];
        for (long long i=1; i<n; i++){
            sum[i] = a[i] + sum[i-1];
        }

        bool works = true;
        for (long long i=0; i<n; i++){
            if (sum[i] < ((i+1) * (i+2))/2){
                works = false;
                break;
            }
        }

        if (works == true) cout << "YES" << "\n";
        else cout << "NO" << "\n";
    }
}
```
- [Stepan and Permutation](https://codeforces.com/contest/2244/problem/C)
```
#include <bits/stdc++.h>
using namespace std;

int main(){
    int t;
    cin >> t;
    while (t--){
        int n, x, y;
        cin >> n >> x >> y;

        int p[n+1];
        for (int i=1; i<=n; i++) cin >> p[i];

        vector<vector<int>> adj(n+1);
        for (int i=1; i<=n-x; i++){
            adj[i].push_back(i+x);
            adj[i+x].push_back(i);
        }

        for (int i=1; i<=n-y; i++){
            adj[i].push_back(i+y);
            adj[i+y].push_back(i);
        }

        int comp[n+1];
        for (int i=0; i<=n; i++) comp[i] = 0;

        int num = 0;

        for (int i=1; i<=n; i++){
            if (comp[i] != 0) continue;
            num++;
            queue<int> q;
            q.push(i);
            comp[i] = num;

            while (q.empty() == false){
                int x = q.front();
                q.pop();

                for (int y: adj[x]){
                    if (comp[y] == 0){
                        comp[y] = num;
                        q.push(y);
                    }
                }
            }
        }

        bool works = true;
        for (int i=1; i<=n; i++){
            if (comp[i] != comp[p[i]]){
                works = false;
                break;
            }
        }
        
        if (works == true) cout << "YES" << "\n";
        else cout << "NO" << "\n";
    }
}
```
- [Yaroslav and Productivity](https://codeforces.com/contest/2244/problem/D)
```
#include <bits/stdc++.h>
using namespace std;

int main(){
    int t;
    cin >> t;
    while (t--){
        int n, m;
        cin >> n >> m;

        vector<long long> a(n+1);
        for (int i = 1; i <= n; i++) cin >> a[i];
        vector<long long> b(m+1);
        for (int i = 1; i <= m; i++) cin >> b[i];

        vector<int> avail(n+1, 0);
        for (int i = 1; i <= m; i++) avail[b[i]] = 1;

        vector<array<long long,2>> dp(n+2);
        dp[n+1][0] = 0;
        dp[n+1][1] = -1e15;

        for (int i = n; i >= 1; i--){
            if (avail[i] == 0){
                dp[i][0] = a[i] + dp[i+1][0];
                dp[i][1] = -a[i] + dp[i+1][1];
            }
            else{
                long long best = max(dp[i+1][0], dp[i+1][1]);
                dp[i][0] = a[i] + best;
                dp[i][1] = -a[i] + best;
            }
        }

        cout << max(dp[1][0], dp[1][1]) << "\n";
    }
}
```
- [Masha and the Garland](https://codeforces.com/contest/2244/problem/E)
```
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;

    while (t--) {
        int n, q;
        cin >> n >> q;

        char s[n+1];
        for (int i=1; i<=n; i++) cin >> s[i];

        int bad0[n + 1], bad1[n + 1];
        int pref0[n + 1], pref1[n + 1];
        int block0[n + 1], block1[n + 1];

        pref0[0] = pref1[0] = 0;
        block0[0] = block1[0] = 0;
        bad0[0] = bad1[0] = 0;

        for (int i = 1; i <= n; i++) {

            char c0, c1;
            if (i%2 == 0){
                c0 = '0';
                c1 = '1';
            }
            else{
                c0 = '1';
                c1 = '0';
            }

            if (s[i] != c0) bad0[i] = 1;
            else bad0[i] = 0;
            if (s[i] != c1) bad1[i] = 1;
            else bad1[i] = 0;

            pref0[i] = pref0[i - 1] + bad0[i];
            pref1[i] = pref1[i - 1] + bad1[i];

            block0[i] = block0[i - 1];
            if (bad0[i]==1 && bad0[i - 1]==0) block0[i]++;

            block1[i] = block1[i - 1];
            if (bad1[i]==1 && bad1[i - 1]==0) block1[i]++;
        }

        while (q--) {

            int l, r, k;
            cin >> l >> r >> k;

            int b0 = block0[r] - block0[l - 1];
            if (l > 1 && bad0[l] && bad0[l - 1]) b0++;

            int b1 = block1[r] - block1[l - 1];
            if (l > 1 && bad1[l] && bad1[l - 1]) b1++;

            if (min(b0, b1) <= k) cout << "YES\n";
            else cout << "NO\n";
        }
    }
}
```
