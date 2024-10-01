classDiagram
direction BT
class CUSTOMER {
   nvarchar(25) CUSTOMERFIRSTNAME
   nvarchar(25) CUSTOMERLASTNAME
   nvarchar(15) CUSTOMERPHONE
   nvarchar(50) CUSTOMERSTREET
   nvarchar(25) CUSTOMERCITY
   char(2) CUSTOMERSTATE
   nvarchar(9) CUSTOMERZIP_CODE
   nvarchar(50) CUSTOMEREMAIL
   nvarchar(5) MEMBERID
   nvarchar(5) CUSTOMERID
}
class DEPARTMENT {
   nvarchar(30) DEPARTMENTNAME
   nvarchar(5) DEPARTMENTID
}
class Discount {
   nvarchar(5) Discount_id
   nvarchar(25) Name
   nvarchar(25) Description
   int Type
}
class Discount {
   nvarchar(25) Name
   nvarchar(25) Description
   int Type
   nvarchar(5) Discount_id
}
class Dtype {
   int Value
   int Percentage
   nvarchar(5) Free_prod_id
   int Type
}
class EMPLOYEE {
   nvarchar(25) EMPLOYEEFIRSTNAME
   nvarchar(25) EMPLOYEELASTNAME
   nvarchar(15) EMPLOYEEPHONE
   nvarchar(50) EMPLOYEESTREET
   nvarchar(25) EMPLOYEECITY
   char(2) EMPLOYEESTATE
   nvarchar(9) EMPLOYEEZIP_CODE
   nvarchar(50) EMPLOYEEEMAIL
   date EMPLOYEEDOB
   date EMPLOYEEHIROFEDATE
   nvarchar(15) EMPLOYEESSN
   char(2) EMPLOYEESTATUS
   nvarchar(5) EMPMANAGERID
   nvarchar(5) STOREID
   nvarchar(5) EMPLOYEEID
}
class GasOrder {
   nvarchar(5) ID
   int Pump_Num
   nvarchar(5) TransId
}
class GasOrder {
   int Pump_Num
   nvarchar(5) TransId
   nvarchar(5) ID
}
class HOLD {
   int QTYHELD
   nvarchar(5) STOREID
   nvarchar(5) PRODUCTID
}
class In_Store {
   nvarchar(3) GasFlag
   nvarchar(5) GOrderId
   nvarchar(5) id
}
class MEMBER {
   nvarchar(9) MEMBERDRIVERLICENSENO
   date ENROLLOFDATE
   nvarchar(5) MEMBERID
}
class NUTRITION {
   int TTLCALORIES
   int FATCALORIES
   int TTLFAT
   int SATFAT
   int TRANSFAT
   int CHOLESTEROL
   int SODIUM
   int TTLCARBOHYDRATES
   int DIETARYFIBER
   int SUGARS
   int PROTEIN
   nvarchar(5) PRODUCTID
}
class Online {
   nvarchar(5) id
}
class PRODUCT {
   nvarchar(25) PRODUCTNAME
   nvarchar(50) PRODUCTDESCRIPTION
   nvarchar(2) SUPPLIERID
   nvarchar(5) STOREID
   nvarchar(5) DEPARTMENTID
   nvarchar(5) PRODUCTID
}
class REVIEW {
   date REVIEWDATE
   char(1) REVIEWRATING
   nvarchar(250) REVIEWDESCRIPTION
   nvarchar(5) PRODUCTID
   nvarchar(5) CUSTOMERID
   nvarchar(5) REVIEWID
}
class STORE {
   nvarchar(50) STORESTREET
   nvarchar(25) STORECITY
   char(2) STORESTATE
   nvarchar(9) STOREZIP_CODE
   nvarchar(5) MANAGERID
   nvarchar(5) STOREID
}
class SUPPLIER {
   nvarchar(20) SUPPLIERNAME
   nvarchar(15) SUPPLIERPHONE
   nvarchar(50) SUPPLIERSTREET
   nvarchar(20) SUPPLIERCITY
   char(2) SUPPLIERSTATE
   nvarchar(9) SUPPLIERZIP_CODE
   nvarchar(50) EMPLOYEEEMAIL
   nvarchar(2) SUPPLIERID
}
class TAX {
   decimal(4,2) TAX_RATE
   char(2) STATE
}
class TransType {
   nvarchar(5) id
}
class order {
   date Order_Date
   timestamp Order_TIME
   nvarchar(5) CUSTOMERID
   decimal(10,2) Sub_total_amount
   decimal(10,2) Tax_amount
   decimal(10,2) Discount_amount
   nvarchar(5) Discount_id
   decimal(10,2) Total_amount
   nvarchar(5) PRODUCTID
   nvarchar(5) online_transaction_id
   nvarchar(5) In_store_transcation_id
   int Quantity
   nvarchar(5) TransId
   nvarchar(5) Order_id
}
class order_line {
   nvarchar(5) PRODUCTID
   int Quantity
   nvarchar(5) order_id
   nvarchar(5) Order_Line_item_id
}
class price_history {
   date EndDate
   decimal(8,2) Unit_price
   nvarchar(5) STOREID
   nvarchar(5) PRODUCTID
   date StartDATE
}

CUSTOMER  -->  MEMBER : MEMBERID
Discount  -->  Dtype : Type
Dtype  -->  PRODUCT : Free_prod_id:PRODUCTID
EMPLOYEE  -->  EMPLOYEE : EMPMANAGERID:EMPLOYEEID
EMPLOYEE  -->  STORE : STOREID
GasOrder  -->  TransType : TransId:id
HOLD  -->  PRODUCT : PRODUCTID
HOLD  -->  STORE : STOREID
In_Store  -->  GasOrder : GOrderId:ID
NUTRITION  -->  PRODUCT : PRODUCTID
PRODUCT  -->  DEPARTMENT : DEPARTMENTID
PRODUCT  -->  STORE : STOREID
PRODUCT  -->  SUPPLIER : SUPPLIERID
REVIEW  -->  CUSTOMER : CUSTOMERID
REVIEW  -->  PRODUCT : PRODUCTID
STORE  -->  EMPLOYEE : MANAGERID:EMPLOYEEID
order  -->  CUSTOMER : CUSTOMERID
order  -->  Discount : Discount_id
order  -->  In_Store : In_store_transcation_id:id
order  -->  Online : online_transaction_id:id
order  -->  TransType : TransId:id
order_line  -->  PRODUCT : PRODUCTID
price_history  -->  PRODUCT : PRODUCTID
price_history  -->  STORE : STOREID
