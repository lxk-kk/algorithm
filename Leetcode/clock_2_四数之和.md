**题目描述**

```
给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。
```

**示例**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

**测试用例**

```java
public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    int target;
    long start;
    long end;
    int size;
    Random random = new Random();
    while (true) {
        int len = in.nextInt();
        int[] arr = new int[len];
        int mid = len >> 1;
        // target = mid >> 1;
        target = 0;
        for (int i = 0; i < len; i++) {
            // 随机数据测试
            // arr[i] = random.nextInt(len << 2);

            // 密集性测试
            if (i < (len >> 1)) {
                arr[i] = i - mid + 1;
            } else {
                arr[i] = i - mid;
            }
        }
        start = System.currentTimeMillis();
        size = fourSum(arr, target).size();
        end = System.currentTimeMillis() - start;

        System.out.print("fast_size = " + size);
        System.out.println(" \t 耗时=" + end);

        start = System.currentTimeMillis();
        size = fourSum_1(arr, target).size();
        end = System.currentTimeMillis() - start;

        System.out.print("slow_size = " + size);
        System.out.println(" \t 耗时=" + end);

    }
}
```

**解答**

解1：**排序 + 三重循环 + 双指针 + 减枝**

```java
/**
 * 排序 + 三重循环 + 双指针 + 减枝
 *
 * @param nums
 * @param target
 * @return
 */
public static List<List<Integer>> fourSum_1(int[] nums, int target) {

    List<List<Integer>> result = new ArrayList<>();
    if (nums.length < 4) {
        return result;
    }
    int len = nums.length - 1;
    Arrays.sort(nums);
    for (int i = 0; i < nums.length - 3; ++i) { // 一重循环
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        if (nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target) {
            // 减枝
            break;
        }
        if (nums[i] + nums[len] + nums[len - 1] + nums[len - 2] < target) {
            // 减枝
            continue;
        }
        for (int j = i + 1; j < nums.length - 2; ++j) { // 二重循环
            if (j > i + 1 && nums[j] == nums[j - 1]) {
                continue;
            }
            if (nums[i] + nums[j] + nums[j + 1] + nums[j + 2] > target) {
                // 减枝
                break;
            }
            if (nums[i] + nums[j] + nums[len - 1] + nums[len] < target) {
                // 减枝
                continue;
            }
            int st = j + 1;
            int end = nums.length - 1;
            while (st < end) { // 三重循环
                int sum = nums[i] + nums[j] + nums[st] + nums[end];
                if (sum == target) {
                    result.add(Arrays.asList(nums[i], nums[j], nums[st], nums[end]));
                }
                if (sum > target) {
                    do {
                        end--;
                    } while (st < end && nums[end] == nums[end + 1]);
                } else {
                    do {
                        st++;
                    } while (st < end && nums[st] == nums[st - 1]);
                }
            }
        }
    }
    return result;
}
```

解2：**排序 + 四指针 + 减枝**

```java
/**
 * 四指针 + 减枝
 *
 * @param nums
 * @param target
 * @return
 */
public static List<List<Integer>> fourSum(int[] nums, int target) {
    List<List<Integer>> result = new ArrayList<>();
    if (nums.length < 4) {
        return result;
    }
    Arrays.sort(nums);
    int st = 0;
    int end = nums.length - 1;

    // 初始化
    int est = -1;
    int eend = -1;
    int equalsing = -1;

    while (st + 1 < end - 1) { // 一重循环
        int sumMax = nums[st] + nums[end - 2] + nums[end - 1] + nums[end];
        int sumMin = nums[st] + nums[st + 1] + nums[st + 2] + nums[end];
        boolean equals = false;

        // 通过 sumMax 和 sumMin 做减枝
        if (sumMax >= target && target >= sumMin) {
            int pre = st + 1;
            int last = end - 1;

            while (pre < last) { // 二重循环
                int sum = nums[st] + nums[end] + nums[pre] + nums[last];
                if (sum == target) {
                    result.add(Arrays.asList(nums[st], nums[pre], nums[last], nums[end]));
                }
                if (sum >= target) {
                    do {
                        last--;
                    } while (pre < last && nums[last] == nums[last + 1]);
                } else {
                    do {
                        pre++;
                    } while (pre < last && nums[pre] == nums[pre - 1]);
                }
            }
            if (equalsing == -1) { // 需要复位：先 st++ end不变，后 st不变 end--，最后 st、end 复位，且同时 st++ end--
                est = st;
                eend = end;
                equalsing = 0;
            }
            equals = true;
        }
        if (sumMax < target || equalsing == 0 || (equals && equalsing == 1)) {
            if (equalsing == 0) { // 先 st++
                // st = est; 隐含
                equalsing = 1;
            }

            if (equalsing == 2) { // 复位 st、end，且 st++、end--
                equalsing = -1;
                st = est;
                end = eend;

                do {
                    st++;
                } while (st < end && nums[st] == nums[st - 1]);
                do {
                    end--;
                } while (st < end && nums[end] == nums[end + 1]);
                continue;
            }

            do {
                st++;
            } while (st < end && nums[st] == nums[st - 1]);
            
        } else if (sumMin > target || (equals && equalsing == 2)) {
            if (equalsing == 1) { // 后 end--
                st = est;
                equalsing = 2;
            }

            do {
                end--;
            } while (st < end && nums[end] == nums[end + 1]);
        }
    }
    return result;
}
```

