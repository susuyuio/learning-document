# bash-shell

## cmd

- echo "text"
- echo 'text'
- echo text
- printf "test"
- echo ${var}
- echo $var
- echo "$var"
- echo ${#vartest}  // vartest.length
- let result=no1+no2 ; echo $result
- result=$[no1+$no2+5]
- result=`expr 3 + 4`
- echo "4 * 0.56" | bc
- bc可以接受操作控制前缀,这些前缀之间使用分号分隔
  - 设定小数精度
    - echo "scale=2;22/7" | bc  // 3.14
    - echo "scale=3;22/7" | bc  // 3.142
  - 进制转换
    - no=100 ; echo "obase=2;$no" | bc  // 1100100
  - 计算平方以及平方根
    - echo "sqrt(100)" | bc #Square root
    - echo "10^10" | bc #Square