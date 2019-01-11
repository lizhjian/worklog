# worklog
## 2019-01-11
* []确认交割协议

## 2019-01-09
* 解约物业交割完成listener
    []生成结算前的校验
      [x]完成开发
      []校验物业交割/物品交割是否已完成接口
      []自测
    []计算结算金额
    []生成结算审核（审核系统做兼容）
    []生成物业交割协议pdf
## 2019-01-08
* []和刘威对定时任务工作量
     给威哥出功能点文档
* []把业务上不清楚的点记录整理下来
     
## 2019-01-04
* []计算公式
A.预算友家整租规则：
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
         []根据取消解约时间查询对应的付款计划，查看是哪一期 再根据对应的is_vancacy
         []外置后置
           [x]空置期内解约
              [x]佣金服务费=（当前管理年度空置期结束天数-解约预算时间）*月租金/30    付款计划的isvancay=1则认为是空置期内解约，用end_cycle-breakTime * month_house_rent
           [x]控制器外解约
              [x]佣金服务费=空置期时长*月租金/30
                [???]哪个年度的空置期时长,空置期都是跨年度的
         []内置后置
           [??? 该管理年度]该管理年度内如果存在服务费抵扣付款计划且已支付(只看payment_status字段???)，则该款项为0
           [??? 该管理年度]该管理年度内如果存在服务费抵扣付款计划且未支付，则该款项取服务费抵扣字段值
         []合同扣款单总金额：该合同所有未付款的扣款单和待扣款单金额：由财务取+还款计划   
     [???]自如退还业主租金：
     start_cycle    
B.特殊规则（业主年付贷）
     [??? 当个管理周期内已履约整月数]*业主退还公司租金：业主退还公司租金=发起预算时（10-发起预算时当个管理周期内已履约整月数）*月租金+（30-已履约零天数）*（月租金/30），g=1。空置期内预算，g=0；非空置期内（本年度房租已支付），g=1s              
     [??? 承租合同 ]b*业主与客户之间的违约金：业主与客户之间的违约金=发起预算是出房月租金（从出房取）。业主违约：有承租合同时违约，a=1，没有承租合同时违约，a=0；自如违约：a=0

C.直租豪宅类规则（无年付贷）：
     [???当期租金,如果是空置期咋办]d*业主退还自如的租金：业主退还自如的租金=自如已支付当期租金-当期客户实际居住整月数*收房月租金-当期客户实际居住零天数*日租金。日租金=收房月租金/30。“当前”指发起解约当日
 
解约结算：
     友家整租规则：
     [???最后一个租客解约不在空置期怎么办]d*(装修折旧费+配置折旧费)：装修折旧费+配置折旧费=（新收配置订单的装修成本+配置成本）*结算时（非空置时为最后一个租客解约当天，空置时为业主违约金当日）合同未履约自然天数/合同履约总自然天数。任何解约类型，d=1        


撤销解约：
     [等额客户违约金  == 自如和客户的违约金???]b*客户的违约金：客户的违约金=预算等额客户违约金。无论申请撤销原来在住的客户是否已经解约，均b=1
     [???系数]e：折扣系数。通过链家卖房，e=0.8；政府事务中心特批，e≥0.7
     
          
## 2019-01-03
* []计算公式
* []zo配置
    * [x]生产环境
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