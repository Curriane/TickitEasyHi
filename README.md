# TickitEasyHi
TickitEasyHibernate

# Java 集合與 JDBC 練習

---

## 練習一：模擬骰子投擲

### 題目：
模擬 2 顆骰子投擲 20 次，統計出現的點數總和，並輸出點數及次數（以 * 表示）。

### 答案：
```java
import java.util.Random;

public class DiceArray {
    public static void main(String[] args) {
        int[] diceSums = new int[13];
        Random random = new Random();

        for (int i = 0; i < 20; i++) {
            int dice1 = random.nextInt(6) + 1;
            int dice2 = random.nextInt(6) + 1;
            int sum = dice1 + dice2;
            diceSums[sum]++;
        }

        for (int i = 2; i <= 12; i++) {
            if (diceSums[i] > 0) {
                System.out.print(i + " ");
                for (int j = 0; j < diceSums[i]; j++) {
                    System.out.print("*");
                }
                System.out.println();
            }
        }
    }
}
```

---

## 練習二：隨機生成不重複數字 (範圍 1~10)

### 題目：
生成 5 個不重複的隨機數字（範圍 1~10），並排序輸出。

### 答案：
```java
import java.util.Set;
import java.util.TreeSet;
import java.util.Random;

public class RandomNum {
    public static void main(String[] args) {
        Set<Integer> numbers = new TreeSet<>();
        Random random = new Random();

        while (numbers.size() < 5) {
            int num = random.nextInt(10) + 1;
            numbers.add(num);
        }

        System.out.println(numbers);
    }
}
```

---

## 練習三：隨機生成不重複數字 (範圍 1~100)

### 題目：
生成 15 個不重複的隨機數字（範圍 1~100），並排序與計算平均值。

### 答案：
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Random;

public class RandomList {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<>();
        Random random = new Random();

        while (numbers.size() < 15) {
            int num = random.nextInt(100) + 1;
            if (!numbers.contains(num)) {
                numbers.add(num);
            }
        }

        Collections.sort(numbers);
        System.out.println("排序後的數字: " + numbers);

        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        double average = sum / 15.0;
        System.out.println("平均值: " + average);
    }
}
```

---

## JDBC 練習

---

### 練習四：根據主鍵查詢

### 題目：
查詢製造商為 Toyota 且型號為 R8 的車輛，並將結果存入 Map。

### 答案：
```java
import java.sql.*;
import java.util.HashMap;
import java.util.Map;

public class QueryByPK {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/STUDENT";
        String user = "root";
        String password = "password";

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            String query = "SELECT * FROM CARS WHERE MANUFACTURER = ? AND TYPE = ?";
            PreparedStatement stmt = conn.prepareStatement(query);
            stmt.setString(1, "Toyota");
            stmt.setString(2, "R8");

            ResultSet rs = stmt.executeQuery();
            Map<String, Object> carData = new HashMap<>();

            if (rs.next()) {
                carData.put("MANUFACTURER", rs.getString("MANUFACTURER"));
                carData.put("TYPE", rs.getString("TYPE"));
                carData.put("PRICE", rs.getDouble("PRICE"));
                carData.put("MIN_PRICE", rs.getDouble("MIN_PRICE"));
            }

            System.out.println(carData);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

---

---

## 練習三：員工 Map 操作

### 題目：
1. 建立 `EmpMap.java`，存放以下行編 (String) 與姓名 (String)：
   - 行編: 12345，姓名: 王大同
   - 行編: 02345，姓名: 李中明
   - 行編: 82345，姓名: 林小白
2. 使用 forEach 迴圈輸出每一組員工的行編與姓名。
3. 輸出行編 12345 的員工姓名。

### 答案：
```java
import java.util.HashMap;
import java.util.Map;

public class EmpMap {
    public static void main(String[] args) {
        Map<String, String> employees = new HashMap<>();
        employees.put("12345", "王大同");
        employees.put("02345", "李中明");
        employees.put("82345", "林小白");

        employees.forEach((id, name) -> System.out.println(id + " -> " + name));
        System.out.println("行編 12345 的員工姓名: " + employees.get("12345"));
    }
}
```

---

## 練習四：歌曲 Map 操作

### 題目：
1. 建立 `SongMap.java`，存放以下歌手與歌曲名稱：
   - Coldplay -> Yellow
   - Adele -> Skyfall
   - Lady Gaga -> Shallow
2. 按字母排序後，使用 forEach 輸出每組 `Singer` 與 `Song`。
3. 輸出所有歌手與歌曲名稱。

### 答案：
```java
import java.util.Map;
import java.util.TreeMap;

public class SongMap {
    public static void main(String[] args) {
        Map<String, String> songs = new TreeMap<>();
        songs.put("Coldplay", "Yellow");
        songs.put("Adele", "Skyfall");
        songs.put("Lady Gaga", "Shallow");

        songs.forEach((singer, song) -> 
            System.out.println("Singer: " + singer + ", Song: " + song));

        System.out.println("所有歌手: " + songs.keySet());
        System.out.println("所有歌曲: " + songs.values());
    }
}
```

---

## 練習五：例外處理 - 檔案讀取

### 題目：
1. 模擬檔案讀取失敗。
2. 使用 try-catch-finally，輸出例外訊息與 Finally Block。

### 答案：
```java
import java.io.FileReader;
import java.io.IOException;

public class ExceptionDemo {
    public static void main(String[] args) {
        try {
            FileReader file = new FileReader("D:/testException.txt");
        } catch (IOException e) {
            System.out.println("Catch Block, Error Message: " + e.getMessage());
        } finally {
            System.out.println("Finally Block");
        }
    }
}
```

---

## 練習六：例外處理 - 奇偶數檢測

### 題目：
1. 隨機生成 1~100 的數字，檢測奇數或偶數。
2. 若為偶數，拋出 Exception 並捕捉。

### 答案：
```java
import java.util.Random;

public class ExceptionHandle {
    public static void main(String[] args) {
        Random random = new Random();
        int number = random.nextInt(100) + 1;

        try {
            if (number % 2 == 0) {
                throw new Exception("偶數: " + number);
            }
            System.out.println("奇數: " + number);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


