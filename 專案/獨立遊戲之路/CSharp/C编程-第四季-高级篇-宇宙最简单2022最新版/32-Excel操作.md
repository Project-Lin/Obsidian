---
Daily: '[[20240331]]'
tags:
  - csharp
up: "[[專案/獨立遊戲之路/CSharpRoad|CSharpRoad]]"
課程: "[[C编程-第四季-高级篇-宇宙最简单2022最新版]]"
date: 2024-03-31
---


```csharp  file:Program
using System.Data;
using System.Data.OleDb;

namespace _32_Excel操作
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //檔案名稱 放到默認資料夾
            string fileName = "Test.xlsx";
            //Excel 連接語句 只要把fileName 放到Data Source位置
            string connectionStr = $"Provider=Microsoft.ACE.OLEDB.12.0;Data Source={fileName};Extended Properties='Excel 12.0;HDR=YES;'";
            //創建oledb連接 使用連接語句
            OleDbConnection connection = new OleDbConnection(connectionStr);
            //開啟連接
            connection.Open();  

			//SQL 查詢字串
            string sql = "select * from [工作表1$]";
			//DataAdapter(查詢字串,連接)
            OleDbDataAdapter adapter = new OleDbDataAdapter(sql,connection);
			//創建DataSet保存所有數據
            DataSet ds = new DataSet(); 
			//導入
            adapter.Fill(ds);
			//關閉連接
            connection.Close();
			
            //dataset 保存表格集合
            DataTableCollection tablecollect = ds.Tables;
			//提取第一份表格
            DataTable table = tablecollect[0];

            //取得表格的每一行 放到集合
            DataRowCollection dataRowCollection = table.Rows;
			//遍歷每一行 因為每行數據有7個 所以內循環7次就換行
            foreach (DataRow row in dataRowCollection)
            {
                for(int i = 0;i<7;i++)
                {
                    Console.Write(row[i] + " ");
                }
                Console.WriteLine();
            }


        }

      
    }
}

```

# Excel 操作

## 檔案操作

### 檔案名稱設置
- FileName: 設置 Excel 檔案名稱為 "Test. Xlsx"

## Excel 導入流程

### 創建連接字串
- ConnectionStr: 設置 OleDb 連接字串，指定 Excel 檔案的路徑及其屬性。

### 創建 OleDb 連接
- OleDbConnection: 用於建立到 Excel 檔案的連接。

### 開啟連接
- Connection.Open (): 打開與 Excel 檔案的連接。

### SQL 查詢字串
- Sql: 定義 SQL 查詢以從 Excel 檔案中選擇資料。

### 創建 DataAdapter
- OleDbDataAdapter: 用於執行 SQL 查詢並從 Excel 檔案中擷取資料。

### 創建 DataSet 保存所有數據
- DataSet: 用於保存從 Excel 檔案中擷取的所有資料。

### 導入數據
- Adapter.Fill (ds): 將從 Excel 檔案中獲取的資料填充到 DataSet 中。

### 關閉連接
- Connection.Close (): 關閉與 Excel 檔案的連接。

## 數據處理

### 提取表格集合
- DataTableCollection: 保存 DataSet 中的表格集合。

### 提取第一份表格
- DataTable: 代表 DataSet 中的一個表格。

### 取得表格的每一行
- DataRowCollection: 保存 DataTable 中的每一行資料。

### 遍歷每一行，並輸出
- Foreach: 用於遍歷 DataRowCollection 中的每一行資料並進行處理。
