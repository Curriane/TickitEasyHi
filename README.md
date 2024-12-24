# TickitEasyHi
TickitEasyHibernate



實作練習 5 - 日期相關操作

需求：輸入 1-12 的整數，顯示該月的完整日曆。

程式碼

package com.cathaybk.practice;

import java.time.LocalDate;
import java.time.format.TextStyle;
import java.util.Locale;
import java.util.Scanner;

public class CalendarApp {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // 提示使用者輸入月份
        System.out.print("輸入介於 1-12 間的整數 m: ");
        int month = scanner.nextInt();

        // 取得當前年份
        int year = LocalDate.now().getYear();

        // 檢查輸入是否有效
        if (month < 1 || month > 12) {
            System.out.println("輸入的月份無效，請輸入 1 到 12 的數字！");
            return;
        }

        // 當月的第一天
        LocalDate firstDayOfMonth = LocalDate.of(year, month, 1);
        // 當月的最後一天
        int daysInMonth = firstDayOfMonth.lengthOfMonth();

        // 顯示標題
        System.out.printf("%d年%d月%n", year, month);
        System.out.println("日 一 二 三 四 五 六");

        // 計算第一天是星期幾
        int dayOfWeek = firstDayOfMonth.getDayOfWeek().getValue();
        dayOfWeek = dayOfWeek % 7; // 調整為從星期日開始

        // 印出空格
        for (int i = 0; i < dayOfWeek; i++) {
            System.out.print("   ");
        }

        // 印出日期
        for (int day = 1; day <= daysInMonth; day++) {
            System.out.printf("%2d ", day);
            if ((day + dayOfWeek) % 7 == 0) { // 換行
                System.out.println();
            }
        }
    }
}

執行結果

輸入 5 時的輸出：

2024年5月
日 一 二 三 四 五 六
         1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30 31

實作練習 6 - 集合資料排序

需求：讀取 cars.csv，並依據價格進行降序排列。

程式碼

package com.cathaybk.practice;

import java.io.*;
import java.util.*;

public class CarsSortApp {
    public static void main(String[] args) {
        List<Map<String, String>> carsList = new ArrayList<>();
        String inputFile = "cars.csv";

        // 讀取資料
        try (BufferedReader reader = new BufferedReader(new FileReader(inputFile))) {
            String line;
            // 跳過標題行
            reader.readLine();
            while ((line = reader.readLine()) != null) {
                String[] data = line.split(",");
                Map<String, String> car = new HashMap<>();
                car.put("Manufacturer", data[0]);
                car.put("Type", data[1]);
                car.put("Price", data[2]);
                carsList.add(car);
            }
        } catch (IOException e) {
            System.err.println("讀取檔案失敗：" + e.getMessage());
            return;
        }

        // 排序
        carsList.sort((car1, car2) -> {
            double price1 = Double.parseDouble(car1.get("Price"));
            double price2 = Double.parseDouble(car2.get("Price"));
            return Double.compare(price2, price1); // 降序
        });

        // 顯示結果
        carsList.forEach(car -> System.out.println(car));
    }
}

執行結果

輸入的 cars.csv 內容：

Manufacturer,Type,Price
Audi,Sedan,30000
BMW,SUV,50000
Toyota,Compact,20000

輸出：

{Manufacturer=BMW, Type=SUV, Price=50000}
{Manufacturer=Audi, Type=Sedan, Price=30000}
{Manufacturer=Toyota, Type=Compact, Price=20000}

實作練習 7 - 資料庫操作

需求：實作 CRUD 操作。

程式碼

package com.cathaybk.practice;

import java.sql.*;
import java.util.Scanner;

public class DatabaseApp {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // 資料庫連線資訊
        String url = "jdbc:oracle:thin:@localhost:1521:xe";
        String user = "student";
        String password = "student";

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            System.out.println("請選擇以下指令輸入：select、insert、update、delete");
            String command = scanner.next();

            switch (command) {
                case "select":
                    System.out.println("輸入製造商：");
                    String manufacturer = scanner.next();
                    PreparedStatement selectStmt = conn.prepareStatement("SELECT * FROM CARS WHERE MANUFACTURER = ?");
                    selectStmt.setString(1, manufacturer);
                    ResultSet rs = selectStmt.executeQuery();
                    while (rs.next()) {
                        System.out.println("製造商：" + rs.getString("MANUFACTURER"));
                        System.out.println("類別：" + rs.getString("TYPE"));
                        System.out.println("價格：" + rs.getDouble("PRICE"));
                    }
                    break;

                case "insert":
                    System.out.println("輸入製造商：");
                    manufacturer = scanner.next();
                    System.out.println("輸入類別：");
                    String type = scanner.next();
                    System.out.println("輸入價格：");
                    double price = scanner.nextDouble();
                    PreparedStatement insertStmt = conn.prepareStatement("INSERT INTO CARS (MANUFACTURER, TYPE, PRICE) VALUES (?, ?, ?)");
                    insertStmt.setString(1, manufacturer);
                    insertStmt.setString(2, type);
                    insertStmt.setDouble(3, price);
                    insertStmt.executeUpdate();
                    System.out.println("新增成功");
                    break;

                case "update":
                    System.out.println("輸入要更新的製造商：");
                    manufacturer = scanner.next();
                    System.out.println("輸入新價格：");
                    price = scanner.nextDouble();
                    PreparedStatement updateStmt = conn.prepareStatement("UPDATE CARS SET PRICE = ? WHERE MANUFACTURER = ?");
                    updateStmt.setDouble(1, price);
                    updateStmt.setString(2, manufacturer);
                    updateStmt.executeUpdate();
                    System.out.println("更新成功");
                    break;

                case "delete":
                    System.out.println("輸入要刪除的製造商：");
                    manufacturer = scanner.next();
                    PreparedStatement deleteStmt = conn.prepareStatement("DELETE FROM CARS WHERE MANUFACTURER = ?");
                    deleteStmt.setString(1, manufacturer);
                    deleteStmt.executeUpdate();
                    System.out.println("刪除成功");
                    break;

                default:
                    System.out.println("無效的指令");
            }
        } catch (SQLException e) {
            System.err.println("資料庫操作失敗：" + e.getMessage());
        }
    }
}

執行結果

依據指令執行不同的操作，例如：

請選擇以下指令輸入：select、insert、update、delete
insert
輸入製造商：BMW
輸入類別：SUV
輸入價格：60000
新增成功
