
如果是vlookup的返回值是#NA
则使用INFA公式
```shell
IFNA(#NA, "")
IFNA(VLOOKUP(B2, table_name!A:B, 2, False), "")
```

