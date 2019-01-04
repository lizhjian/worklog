# worklog
## 2019-01-04
* []计算公式
预算友家整租规则：
     [x]a*业主返还自如的押金 
         押金从合同，合同表deposit字段，每份合同只收取一次
     [x]b*客户的违约金  调用出房getRentContractInfoById
         
     [x]c*业主违约金  本年度月租金（bz_hire_contract_payment / month_house_rent）*2 
     
     [x]d*(装修折旧费+配置折旧费)：从配置取  getDecorateConfigureMoneyApi
          Integer noneCarry = DateUtil.dateSubtract(endDate, calculatorBaseInfo.getBreakTime())+1;  //未履行天数
          decorateConfigurMap.put("fitmentCostCalculate", NumUtil.doubleValue(fitmentCost * noneCarry / total,null,2));    //装修类成本
          decorateConfigurMap.put("totalCostCalculate", NumUtil.doubleValue(totalCost * noneCarry / total,null,2));        //配置类成本
          
     [x]e：折扣系数   e=1
     
     [x]f*宽带折旧费
          调用宽带接口  getBroadbandMoneyApi 获取金额 计算折旧费
     [x]g*公司退还业主租金
          ==0
     [x]h*业主退还服务质量保证金  
          PAYMENT_TYPE 结算方式编码,付款分类 rent 表示租金  bail表示服务质量保证金  isVacancy(是否空置)==1 表示租金 
          （bz_hire_contract_payment 本管理年度服务质量保证金 按paynum排序 取第一条服务质量保证金记录）：发起预算时，未支付本年度保证金，或本年度保证金已从租金中抵扣，h=0；已支付本年度保证金、未从租金中抵扣，h=1
          [???]如何判断是本年度
          [???]如何看是已抵扣
             SELECT  t.* FROM  BZ_HIRE_CONTRACT_PAYMENT  t  WHERE t.HIRE_CONTRACT_ID = 29328 AND t.pay_type = 'bail'  ORDER BY  t.PAY_NUM ASC   
             根据payment_status 判断当前服务质量保证金是否已支付???
     [x]i*佣金服务费：应支付金额以发起时为准（看下表）
         []判断空置期方法 getCalculatorBaseInfo
            []根据取消解约时间
          
         合同扣款单总金额：该合同所有未付款的扣款单和待扣款单金额：由财务取+还款计划   
               
## 2019-01-13
* []计算公式
* []zo配置
    * []生产环境
* []java think 
    []看书看完初始化和清理
    []代码练到第3章
* [x]排期再看一遍  
* [x]生成数据库
* [x]整理业主解约枚举  
##2019-01-02
* [x]okr
* []springboot
* []计算公式
* [x]数据库表
     金额采用BigDecimal类型
     账单表，确认和支付是一个动作吗，已确认改成已确认待付清
     线下支付凭证
* [x]配置zo准生产
* []配置zo生产    
##
* []看需求
  []看springboot
##2018-12-27
* [x]看解约需求，设计数据库
* [???]看spring-boot 从第八章开始
##2018-12-26
* []job-commander 配置准生产和生产环境的
   * []准备生产和准生产域名儿
   * []准生产配置14个定时任务
   * []生产配置14个定时任务
* []下午看测试环境如果没有发版催测试
* [x]看解约需求   

##2018-12-25
* []hire-task提测
  [x]准备提测邮件
  [x]准备测试环境job-commander地址
  [x]发布测试环境流程
* [x]优化派单定时任务及同步派单任务  
* []看业主解约需求  
* []不同项目的域名
   hire-task
   hire-api
   hire-newsign
   zo
   
   ○1hire-api 
   ○2hire-task
   ○3hire-newsign
   ○4生效hire-task 的 job-commander任务

    


2018-12-24
[x]将方法都改为工具类中的方法
dateUtil  ErrorCodeMsgUtil  md5 maputil
[x]commonException  生产者抛出异常      消费者没捕捉到
[x]自测修改mongo方法(优化后的派单)
[x]看springboot  第7部分及其以后 看到第八章了
[ ]看解约需求    第二遍没看完
[x]测task-service事物 