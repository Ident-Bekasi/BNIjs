# BNI API SDK - Node.js

This is the Official Node JS API client / library for BNI API.
Please visit [Digital Services](https://digitalservices.bni.co.id/en/) for more information about our product and visit our documentation page at [API Documentation](https://digitalservices.bni.co.id/documentation/public/en) for more technical details.

## 1. Installation

### 1.1 Using NPM

```
npm install --save bni-nodejs
```

### 1.2 Manual Installation

If you are not using NPM, you can clone or [download](https://github.com/bni-api/bni-nodejs/archive/refs/heads/main.zip) this repository.
Then import from `index.js` file.

```javascript
import { BNIClient } from "bni-nodejs";
// const { BNIClient } = require('bni-nodejs'); // legacy way
```

## 2. Usage

### 2.1 Choose an API Product

We have 2 API products you can use:

- [One Gate Payment](#22A-snap) - A solution for a company to integrate its application / system with banking transaction services. [documentation](https://digitalservices.bni.co.id/en/api-one-gate-payment)
- [Snap BI](https://apidevportal.bi.go.id/snap/info) - Integrate with SNAP BI [documentation](https://apidevportal.bi.go.id/snap/api-services)
- [RDL](#22A-snap) - A solution for a company to integrate its application / system with banking transaction services. [documentation](https://digitalservices.bni.co.id/documentation/3%7Cp2p-lending-register-investor)

### 2.2 Client Initialization and Configuration

Get your client key and server key from [Menu - Applications](https://digitalservices.bni.co.id/en/profile-menu/apps)
Create API client object

```javascript
import { BNIClient } from "bni-nodejs";
// const { BNIClient } = require('bni-nodejs'); // legacy way

// Create Core API instance
const client = new BNIClient({
  env: "sandbox", // dev, sandbox or prod
  clientId: "{your-client-id}",
  clientSecret: "{your-client-secret}",
  apiKey: "{your-api-key}",
  apiSecret: "{your-api-secret}",
  appName: "{your-app-name}",
});
```

### 2.2.A One Gate Payment

Create `One Gate Payment` class object

```javascript
import { BNIClient, OneGatePayment } from "bni-nodejs";
// const { BNIClient, OneGatePayment } = require('bni-nodejs'); // legacy way

// Create Client instance
const client = new BNIClient({
  env: "sandbox", // dev, sandbox or prod
  clientId: "{your-client-id}",
  clientSecret: "{your-client-secret}",
  apiKey: "{your-api-key}",
  apiSecret: "{your-api-secret}",
  appName: "{your-app-name}",
});

const ogp = new OneGatePayment(client);
```

Available methods for `One Gate Payment` class

#### Get Balance

```javascript
// return as Promise of Object
const getBalance = await ogp.getBalance({
  accountNo: "113183203",
});
```

#### Get In House Inquiry

```javascript
// return as Promise of Object
const getInHouseInquiry = await ogp.getInHouseInquiry({
  accountNo: "113183203",
});
```

#### Do Payment

```javascript
// return as Promise of Object
const doPayment = await ogp.doPayment({
  customerReferenceNumber: "20170227000000000020", // max 20 char client defined reference number
  paymentMethod: "0", // 0: In-house (intra BNI), 1: RTGS transfer, 2: Kliring transfer
  debitAccountNo: "113183203",
  creditAccountNo: "115471119",
  valueDate: "20170227000000000",
  valueCurrency: "IDR",
  valueAmount: "100500",
  remark: "", // optional
  beneficiaryEmailAddress: "mail@example.com", // optional
  beneficiaryName: "Mr. X", // optional max 50 char (mandatory if paymentMethod 1 / 2)
  beneficiaryAddress1: "Jakarta", // optional max 50 char (mandatory if paymentMethod 1 / 2)
  beneficiaryAddress2: "", // optional max 50 char
  destinationBankCode: "", // optional (mandatory if paymentMethod 1 / 2)
  chargingModelId: "OUR", // OUR: fee will be paid by sender (default), BEN: fee will be paid by beneficary, SHA: fee divided
});
```

#### Get Payment Status

```javascript
// return as Promise of Object
const getPaymentStatus = await ogp.getPaymentStatus({
  customerReferenceNumber: "20170227000000000020", // max 20 char client defined reference number
});
```

#### Get Inter Bank Inquiry

```javascript
// return as Promise of Object
const getInterBankInquiry = await ogp.getInterBankInquiry({
  customerReferenceNumber: "20170227000000000021", // max 20 char client defined reference number
  accountNum: "113183203",
  destinationBankCode: "014",
  destinationAccountNum: "3333333333",
});
```

#### Get Inter Bank Payment

```javascript
// return as Promise of Object
const getInterBankPayment = await ogp.getInterBankPayment({
  customerReferenceNumber: "20170227000000000021", // max 20 char client defined reference number
  amount: "100500",
  destinationAccountNum: "3333333333",
  destinationAccountName:
    "BENEFICIARY NAME 1 UNTIL HERE1BENEFICIARY NAME 2(OPT) UNTIL HERE2",
  destinationBankCode: "014",
  destinationBankName: "BCA",
  accountNum: "115471119",
  retrievalReffNum: "100000000024", // refference number for Interbank Transaction
});
```

#### Hold Amount

```javascript
// return as Promise of Object
const holdAmount = await ogp.holdAmount({
  customerReferenceNumber: "20170504153218296", // max 20 char client defined reference number
  amount: "12007",
  accountNo: "0115476151",
  detail: "", // optional
});
```

#### Hold Amount Release

```javascript
// return as Promise of Object
const holdAmountRelease = await ogp.holdAmountRelease({
  customerReferenceNumber: "20170504153218296", // max 20 char client defined reference number
  amount: "12007",
  accountNo: "0115476151",
  bankReference: "513668", // journal number. you can get this value from hold amount response
  holdTransactionDate: "31052010", // the date when you do the hold transaction
});
```

### 2.2.B Snap BI

Create `Snap BI` class object

```javascript
import { BNIClient, SnapBI } from "bni-nodejs";
// const { BNIClient, SnapBI } = require('bni-nodejs'); // legacy way

// Create Client instance
const client = new BNIClient({
  env: "dev", // dev, sandbox or prod
  clientId: "{your-client-id}",
  clientSecret: "{your-client-secret}",
  apiKey: "{your-api-key}",
  apiSecret: "{your-api-secret}",
  appName: "{your-app-name}",
});

const snap = new SnapBI(client, {
  privateKeyPath: "{your-rsa-private-key-path}",
  channelId: "{your-channel-id}",
});
```

Available methods for `Snap BI` class

#### Balance Inquiry

```javascript
// return as Promise of Object
const balanceInquiry = await snap.balanceInquiry({
  partnerReferenceNo: "202010290000000000002", // optional
  accountNo: "0115476117",
});
```

#### Bank Statement

```javascript
// return as Promise of Object
const bankStatement = await snap.bankStatement({
  partnerReferenceNo: "202010290000000000002", // optional
  accountNo: "0115476117",
  fromDateTime: "2010-01-01T12:08:56+07:00", // optional
  toDateTime: "2011-01-01T12:08:56+07:00", // optional
});
```

#### Internal Account Inquiry

```javascript
// return as Promise of Object
const internalAccountInquiry = await snap.internalAccountInquiry({
  partnerReferenceNo: "2020102900000000000001", // optional
  beneficiaryAccountNo: "0115476151",
});
```

#### Transaction Status Inquiry

```javascript
// return as Promise of Object
const transactionStatusInquiry = await snap.transactionStatusInquiry({
  originalPartnerReferenceNo: "20211213100434", // optional
  originalReferenceNo: "20211220141520", // transaction reference number
  originalExternalId: "20211220141520", // optional
  serviceCode: "36", // SNAP BI service code
  transactionDate: "2021-12-20",
  amount: {
    value: "12500",
    currency: "IDR",
  },
  additionalInfo: {
    deviceId: "123456", // optinal
    channel: "mobilephone", // optinal
  },
});
```

#### Transfer Intra Bank

```javascript
// return as Promise of Object
const **transferIntraBank** = await snap.transferIntraBank({
  partnerReferenceNo: '202201911020300006', // transaction reference number
  amount: {
    value: '12500',
    currency: 'IDR'
  },
  beneficiaryAccountNo: '0115476117',
  beneficiaryEmail: 'mail@example.com', // optional
  currency: 'IDR', // optional
  customerReference: '14045', // optional
  feeType: 'OUR', // OUR: fee will be paid by sender (default), BEN: fee will be paid by beneficary, SHA: fee divided
  remark: '', // optional
  sourceAccountNo: '0115476151',
  transactionDate: '2021-12-13',
  additionalInfo: {
    deviceId: '123456', // optinal
    channel: 'mobilephone' // optinal
  }
});
```

#### Transfer RTGS

```javascript
// return as Promise of Object
const transferRTGS = await snap.transferRTGS({
  partnerReferenceNo: "202201911020300011", // transaction reference number
  amount: {
    value: "150005001",
    currency: "IDR",
  },
  beneficiaryAccountName: "SAN",
  beneficiaryAccountNo: '"3333333333',
  beneficiaryAccountAddress: "Jakarta Barat", // optional
  beneficiaryBankCode: "CENAIDJA",
  beneficiaryBankName: "PT. BANK CENTRAL ASIA Tbk.", // optional
  beneficiaryCustomerResidence: "1",
  beneficiaryCustomerType: "1",
  beneficiaryEmail: "mail@example.com", // optional
  currency: "IDR", // optional
  customerReference: "202201911020300006",
  feeType: "OUR", // OUR: fee will be paid by sender (default), BEN: fee will be paid by beneficary, SHA: fee divided
  kodePos: "12550", // optional
  recieverPhone: "08123456789", // optional
  remark: "", // optional
  senderCustomerResidence: "1", // optional
  senderCustomerType: "1", // optional
  senderPhone: "08123456789", // optional
  sourceAccountNo: "0115476151",
  transactionDate: "2022-01-25",
  additionalInfo: {
    deviceId: "123456", // optinal
    channel: "mobilephone", // optinal
  },
});
```

#### Transfer SKNBI

```javascript
// return as Promise of Object
const transferSKNBI = await snap.transferSKNBI({
  partnerReferenceNo: "202201911020300012", // transaction reference number
  amount: {
    value: "150005001",
    currency: "IDR",
  },
  beneficiaryAccountName: "SAN",
  beneficiaryAccountNo: "3333333333",
  beneficiaryAddress: "Jakarta Barat", // optional
  beneficiaryBankCode: "0140397",
  beneficiaryBankName: "PT. BANK CENTRAL ASIA Tbk.", // optional
  beneficiaryCustomerResidence: "1",
  beneficiaryCustomerType: "1",
  beneficiaryEmail: "mail@example.com", // optional
  currency: "IDR", // optional
  customerReference: "202201911020300006",
  feeType: "OUR", // OUR: fee will be paid by sender (default), BEN: fee will be paid by beneficary, SHA: fee divided
  kodePos: "12550", // optional
  recieverPhone: "08123456789", // optional
  remark: "", // optional
  senderCustomerResidence: "1", // optional
  senderCustomerType: "1", // optional
  senderPhone: "08123456789", // optional
  sourceAccountNo: "0115476151",
  transactionDate: "2022-01-25",
  additionalInfo: {
    deviceId: "123456", // optinal
    channel: "mobilephone", // optinal
  },
});
```

#### External Account Inquiry

```javascript
// return as Promise of Object
const externalAccountInquiry = await snap.externalAccountInquiry({
  beneficiaryBankCode: "002",
  beneficiaryAccountNo: "888801000157508",
  partnerReferenceNo: "2020102900000000000001", // optional
  additionalInfo: {
    deviceId: "123456", // optinal
    channel: "mobilephone", // optinal
  },
});
```

#### Transfer Inter Bank

```javascript
// return as Promise of Object
const transferInterBank = await snap.transferInterBank({
  partnerReferenceNo: "2020102900000000000001", // transaction reference number
  amount: {
    value: "12345678",
    currency: "IDR",
  },
  beneficiaryAccountName: "Yories Yolanda",
  beneficiaryAccountNo: "888801000003301",
  beneficiaryAddress: "Palembang", // optional
  beneficiaryBankCode: "002",
  beneficiaryBankName: "Bank BRI", // optional
  beneficiaryEmail: "mail@example.com", // optional
  currency: "IDR", // optional
  customerReference: "10052019", // optional
  sourceAccountNo: "888801000157508",
  transactionDate: "2019-07-03T12:08:56+07:00",
  feeType: "OUR", // OUR: fee will be paid by sender (default), BEN: fee will be paid by beneficary, SHA: fee divided
  additionalInfo: {
    deviceId: "123456", // optinal
    channel: "mobilephone", // optinal
  },
});
```

<!--  -->

### 2.2.C P2P Lending / RDL

Create `Rdl` class object

```javascript
import { BNIClient, Rdl } from "bni-nodejs";
// const { BNIClient, Rdl } = require('bni-nodejs'); // legacy way

// Create Client instance
const client = new BNIClient({
  env: "sandbox", // dev, sandbox or prod
  clientId: "{your-client-id}",
  clientSecret: "{your-client-secret}",
  apiKey: "{your-api-key}",
  apiSecret: "{your-api-secret}",
  appName: "{your-app-name}",
});

const rdl = new Rdl(client);
```

Available methods for `Rdl` class

#### Register Investor

```javascript
// return as Promise of Object
const registerInvestor = async () => {
  const res = await rdl.registerInvestor({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS",
    uuidFaceRecog: "492F33851D634CFB", //RequestUuid successed value from Face Recognition API (KYC valid)
    title: "01",
    firstName: "Agus", //optional
    middleName: "", //optional
    lastName: "Saputra",
    optNPWP: "1", //“1” or “0” (Default “1”)
    NPWPNum: "001058893408123",
    nationality: "ID", //e.g.: “ID”
    domicileCountry: "ID", //e.g.: “ID”
    religion: "2",
    birthPlace: "Semarang",
    birthDate: "14081982",
    gender: "M", //“M” or “F”
    isMarried: "S",
    motherMaidenName: "Dina Maryati",
    jobCode: "01",
    education: "07",
    idType: "01",
    idNumber: "4147016201959998", //Identity Number (KTP only)
    idIssuingCity: "Jakarta Barat",
    idExpiryDate: "26102099", //ddMMyyyy
    addressStreet: "Jalan Mawar Melati",
    addressRtRwPerum: "003009Sentosa",
    addressKel: "Cengkareng Barat",
    addressKec: "Cengkareng/Jakarta Barat",
    zipCode: "11730",
    homePhone1: "0214", //Area code, e.g. 021 (3 - 4 digit) If not exist, fill with “9999”
    homePhone2: "7459", //Number after area code (min 4 digit) If not exist, fill with “99999999”
    officePhone1: "", //(Optional) Area code, e.g. 021
    officePhone2: "", //(Optional)  Number after area code
    mobilePhone1: "0812", //Operator code, e.g. 0812 (4 digit) If not exist, fill with “0899”
    mobilePhone2: "12348331", //Number after operator code (min 6 digit) If not exist, fill with “999999”
    faxNum1: "", //(Optional) Area code, e.g. 021
    faxNum2: "", //(Optional) Number after area code
    email: "agus.saputra@gmail.com",
    monthlyIncome: "8000000",
    branchOpening: "0259",
    institutionName: "PT. BNI SECURITIES",
    sid: "IDD280436215354",
    employerName: "Salman", //Employer Name / Company Name
    employerAddDet: "St Baker", //Employer street address / home street address
    employerAddCity: "Arrandelle", //Employer city address / home city address
    jobDesc: "Pedagang",
    ownedBankAccNo: "0337109074",
    idIssuingDate: "10122008",
  });
  return res;
};
```

#### Register Investor Account

```javascript
// return as Promise of Object
const registerInvestorAccount = async () => {
  const res = await rdl.registerInvestorAccount({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    cifNumber: "9100749959",
    currency: "IDR", //“IDR” or “USD”
    openAccountReason: "2",
    sourceOfFund: "1",
    branchId: "0259",
  });
  return res;
};
```

#### Inquiry Account Info

```javascript
// return as Promise of Object
const inquiryAccountInfo = async () => {
  const res = await rdl.inquiryAccountInfo({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    accountNumber: "0115476117",
  });
  return res;
};
```

#### Inquiry Account Balance

```javascript
// return as Promise of Object
const inquiryAccountBalance = async () => {
  const res = await rdl.inquiryAccountBalance({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    accountNumber: "0115476117",
  });
  return res;
};
```

#### Inquiry Account History

```javascript
// return as Promise of Object
const inquiryAccountHistory = async () => {
  const res = await rdl.inquiryAccountHistory({
    companyId: "NI001",
    parentCompanyId: "KSEI", //optional
    accountNumber: "0115476117",
  });
  return res;
};
```

#### Payment Using Transfer

```javascript
// return as Promise of Object
const paymentUsingTransfer = async () => {
  const res = await rdl.paymentUsingTransfer({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    accountNumber: "0115476117",
    beneficiaryAccountNumber: "0115471119",
    currency: "IDR", //e.g., “IDR”
    amount: "11500",
    remark: "Test P2PL", //(optional) Recommended for the reconciliation purpose
  });
  return res;
};
```

#### Payment Using Clearing

```javascript
// return as Promise of Object
const paymentUsingClearing = async () => {
  const res = await rdl.paymentUsingClearing({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    accountNumber: "0115476117",
    beneficiaryAccountNumber: "3333333333",
    beneficiaryAddress1: "Jakarta",
    beneficiaryAddress2: "",
    beneficiaryBankCode: "140397",
    beneficiaryName: "Panji Samudra",
    currency: "IDR", //e.g., “IDR”
    amount: "15000",
    remark: "Test kliring", //(optional)Recommended for the reconciliation purpose
    chargingType: "OUR",
  });
  return res;
};
```

#### Payment Using RTGS

```javascript
// return as Promise of Object
const paymentUsingRTGS = async () => {
  const res = await rdl.paymentUsingRTGS({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS",
    accountNumber: "0115476117",
    beneficiaryAccountNumber: "3333333333",
    beneficiaryAddress1: "Jakarta",
    beneficiaryAddress2: "",
    beneficiaryBankCode: "CENAIDJA",
    beneficiaryName: "Panji Samudra",
    currency: "IDR", //e.g., “IDR”
    amount: "150000000",
    remark: "Test rtgs", //(optional)Recommended for the reconciliation purpose
    chargingType: "OUR",
  });
  return res;
};
```

#### Inquiry Payment Status

```javascript
// return as Promise of Object
const inquiryPaymentStatus = async () => {
  const res = await rdl.inquiryPaymentStatus({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    requestedUuid: "E8C6E0027F6E429F",
  });
  return res;
};
```

#### Inquiry Interbank Account

```javascript
// return as Promise of Object
const inquiryInterbankAccount = async () => {
  const res = await rdl.inquiryInterbankAccount({
    companyId: "NI001",
    parentCompanyId: "KSEI", //optional
    accountNumber: "0115476117",
    beneficiaryBankCode: "013",
    beneficiaryAccountNumber: "01300000",
  });
  return res;
};
```

#### Payment Using Interbank

```javascript
// return as Promise of Object
const paymentUsingInterbank = async () => {
  const res = await rdl.paymentUsingInterbank({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    accountNumber: "0115476117",
    beneficiaryAccountNumber: "3333333333", //Get from Inquiry Interbank Account
    beneficiaryAccountName: "KEN AROK",
    beneficiaryBankCode: "014",
    beneficiaryBankName: "BANK BCA", //Get from Inquiry Interbank Account
    amount: "15000",
  });
  return res;
};
```

#### Face Recognition

```javascript
// return as Promise of Object
const faceRecognition = async () => {
  const res = await rdl.faceRecognition({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    firstName: "MOHAMMAD", //optional
    middleName: "BAQER", //optional
    lastName: "ZALQAD",
    idNumber: "0141111121260118", //Identity Number (KTP only)
    birthDate: "29-09-2021",
    birthPlace: "BANDUNG",
    gender: "M", //“M” or “F”
    cityAddress: "Bandung",
    stateProvAddress: "Jawa Barat",
    addressCountry: "ID", //e.g.: “ID”
    streetAddress1: "bandung",
    streetAddress2: "bandung",
    postCodeAddress: "40914",
    country: "ID", //e.g.: “ID”
    selfiePhoto: "29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuP", //Base64 encoded selfie photo
  });
  return res;
};
```

### 2.2.D Fintech Account Service (RDF)

Create `Fintech Account Service (RDF)` class object
```javascript

import { BNIClient, Rdf } from 'bni-nodejs';
// const { BNIClient, Rdf } = require('bni-nodejs'); // legacy way

// Create Client instance
const client = new BNIClient({
  env: 'sandbox', // dev, sandbox or prod
  clientId: '{your-client-id}',
  clientSecret: '{your-client-secret}',
  apiKey: '{your-api-key}',
  apiSecret: '{your-api-secret}',
  appName: '{your-app-name}'
});

const rdf = new Rdf(client);
```
Available methods for `Fintech Account Service (RDF)` class
#### Register Investor
```javascript
// return as Promise of Object
const registerInvestor = async () => {
  const res = await rdl.registerInvestor({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS",
    uuidFaceRecog: "492F33851D634CFB", //RequestUuid successed value from Face Recognition API (KYC valid)
    title: "01",
    firstName: "Agus", //optional
    middleName: "", //optional
    lastName: "Saputra",
    optNPWP: "1", //“1” or “0” (Default “1”)
    NPWPNum: "001058893408123",
    nationality: "ID", //e.g.: “ID”
    domicileCountry: "ID", //e.g.: “ID”
    religion: "2",
    birthPlace: "Semarang",
    birthDate: "14081982",
    gender: "M", //“M” or “F”
    isMarried: "S",
    motherMaidenName: "Dina Maryati",
    jobCode: "01",
    education: "07",
    idType: "01",
    idNumber: "4147016201959998", //Identity Number (KTP only)
    idIssuingCity: "Jakarta Barat",
    idExpiryDate: "26102099", //ddMMyyyy
    addressStreet: "Jalan Mawar Melati",
    addressRtRwPerum: "003009Sentosa",
    addressKel: "Cengkareng Barat",
    addressKec: "Cengkareng/Jakarta Barat",
    zipCode: "11730",
    homePhone1: "0214", //Area code, e.g. 021 (3 - 4 digit) If not exist, fill with “9999”
    homePhone2: "7459", //Number after area code (min 4 digit) If not exist, fill with “99999999”
    officePhone1: "", //(Optional) Area code, e.g. 021
    officePhone2: "", //(Optional)  Number after area code
    mobilePhone1: "0812", //Operator code, e.g. 0812 (4 digit) If not exist, fill with “0899”
    mobilePhone2: "12348331", //Number after operator code (min 6 digit) If not exist, fill with “999999”
    faxNum1: "", //(Optional) Area code, e.g. 021
    faxNum2: "", //(Optional) Number after area code
    email: "agus.saputra@gmail.com",
    monthlyIncome: "8000000",
    branchOpening: "0259",
    institutionName: "PT. BNI SECURITIES",
    sid: "IDD280436215354",
    employerName: "Salman", //Employer Name / Company Name
    employerAddDet: "St Baker", //Employer street address / home street address
    employerAddCity: "Arrandelle", //Employer city address / home city address
    jobDesc: "Pedagang",
    ownedBankAccNo: "0337109074",
    idIssuingDate: "10122008",
  });
  return res;
};
```
#### Face Recognition
```javascript
// return as Promise of Object
const faceRecognition = async () => {
  const res = await rdl.faceRecognition({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    firstName: "MOHAMMAD", //optional
    middleName: "BAQER", //optional
    lastName: "ZALQAD",
    idNumber: "0141111121260118", //Identity Number (KTP only)
    birthDate: "29-09-2021",
    birthPlace: "BANDUNG",
    gender: "M", //“M” or “F”
    cityAddress: "Bandung",
    stateProvAddress: "Jawa Barat",
    addressCountry: "ID", //e.g.: “ID”
    streetAddress1: "bandung",
    streetAddress2: "bandung",
    postCodeAddress: "40914",
    country: "ID", //e.g.: “ID”
    selfiePhoto: "29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuP", //Base64 encoded selfie photo
  });
  return res;
};
```
#### Register Investor Account
```javascript
// return as Promise of Object
const registerInvestorAccount = async () => {
  const res = await rdf.registerInvestorAccount({
    companyId: ' NI001',
    parentCompanyId: 'KSEI', // optional
    cifNumber: '9100749959',
    currency: 'IDR', // “IDR” or “USD”
    openAccountReason: '2',
    sourceOfFund: '1',
    branchId: '0259',
    bnisId: '19050813401',
    sre: 'NI001CX5U00109' 
  });
  return res;
};
```
#### Inquiry Account Info
```javascript
// return as Promise of Object
const inquiryAccountInfo = async () => {
  const res = rdf.inquiryAccountInfo({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117'
  });
  return res;
};
```
#### Inquiry Account Balance
```javascript
// return as Promise of Object
const inquiryAccountBalance = async () => {
  const res = rdf.inquiryAccountBalance({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117'
  });
  return res;
};
```
#### Inquiry Account History
```javascript
// return as Promise of Object
const inquiryAccountHistory = async () => {
  const res = rdf.inquiryAccountHistory({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117'
  });
  return res;
};
```
#### Payment Using Transfer
```javascript
// return as Promise of Object
const paymentUsingTransfer = async () => {
  const res = rdf.paymentUsingTransfer({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117',
    beneficiaryAccountNumber: '0115471119',
    currency: 'IDR', // e.g., “IDR”
    amount: '11500',
    remark: 'Test RDN' // Recommended for the reconciliation purpose
  });
  return res;
};
```
#### Inquiry Payment Status
```javascript
// return as Promise of Object
const inquiryPaymentStatus = async () => {
  const res = rdf.inquiryPaymentStatus({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    requestedUuid: 'E8C6E0027F6E429F'
  });
  return res;
};
```
#### Payment Using Clearing
```javascript
// return as Promise of Object
const paymentUsingClearing = async () => {
  const res = rdf.paymentUsingClearing({
    companyId: 'SANDBOX',
    parentCompanyId: 'STI_CHS', // optional
    accountNumber: '0115476117',
    beneficiaryAccountNumber: '3333333333',
    beneficiaryAddress1: 'Jakarta',
    beneficiaryAddress2: '',
    beneficiaryBankCode: '140397',
    beneficiaryName: 'Panji Samudra',
    currency: 'IDR', // e.g., “IDR”
    amount: '15000',
    remark: 'Test kliring', // //(optional)Recommended for the reconciliation purpose
    chargingType: 'OUR'
  });
  return res;
};
```
#### Payment Using RTGS
```javascript
// return as Promise of Object
const paymentUsingRTGS = async () => {
  const res = rdf.paymentUsingRTGS({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117',
    beneficiaryAccountNumber: '3333333333',
    beneficiaryAddress1: 'Jakarta',
    beneficiaryAddress2: '',
    beneficiaryBankCode: 'CENAIDJA',
    beneficiaryName: 'Panji Samudra',
    currency: 'IDR', // e.g., “IDR” 
    amount: '150000000',
    remark: 'Test rtgs', // Recommended for the reconciliation purpose
    chargingType: 'OUR'
  });
  return res;
};
```
#### Inquiry Interbank Account
```javascript
// return as Promise of Object
const inquiryInterbankAccount = async () => {
  const res = rdf.inquiryInterbankAccount({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117',
    beneficiaryBankCode: '013',
    beneficiaryAccountNumber: '01300000'
  });
  return res;
};
```
#### Payment Using Interbank
```javascript
// return as Promise of Object
const paymentUsingInterbank = async () => {
  const res = rdf.paymentUsingInterbank({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117',
    beneficiaryAccountNumber: '3333333333',
    beneficiaryAccountName: 'KEN AROK', // Get from Inquiry Interbank Account
    beneficiaryBankCode: '014',
    beneficiaryBankName: 'BANK BCA', // Get from Inquiry Interbank Account
    amount: '15000'
  });
  return res;
};
```

### 2.2.E RDN

Create `RDN` class object
```javascript

import { BNIClient, Rdn } from 'bni-nodejs';
// const { BNIClient, Rdn } = require('bni-nodejs'); // legacy way

// Create Client instance
const client = new BNIClient({
  env: 'sandbox', // dev, sandbox or prod
  clientId: '{your-client-id}',
  clientSecret: '{your-client-secret}',
  apiKey: '{your-api-key}',
  apiSecret: '{your-api-secret}',
  appName: '{your-app-name}'
});

const rdn = new Rdn(client);
```
Available methods for `RDN` class

#### Face Recognition
```javascript
// return as Promise of Object
const faceRecognition = async () => {
  const res = await rdn.faceRecognition({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS", //optional
    firstName: "MOHAMMAD", //optional
    middleName: "BAQER", //optional
    lastName: "ZALQAD",
    idNumber: "0141111121260118", //Identity Number (KTP only)
    birthDate: "29-09-2021",
    birthPlace: "BANDUNG",
    gender: "M", //“M” or “F”
    cityAddress: "Bandung",
    stateProvAddress: "Jawa Barat",
    addressCountry: "ID", //e.g.: “ID”
    streetAddress1: "bandung",
    streetAddress2: "bandung",
    postCodeAddress: "40914",
    country: "ID", //e.g.: “ID”
    selfiePhoto: "29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuP", //Base64 encoded selfie photo
  });
  return res;
};
```
#### Register Investor
```javascript
// return as Promise of Object
const registerInvestor = async () => {
  const res = await rdn.registerInvestor({
    companyId: "SANDBOX",
    parentCompanyId: "STI_CHS",
    uuidFaceRecog: "492F33851D634CFB", //RequestUuid successed value from Face Recognition API (KYC valid)
    title: "01",
    firstName: "Agus", //optional
    middleName: "", //optional
    lastName: "Saputra",
    optNPWP: "1", //“1” or “0” (Default “1”)
    NPWPNum: "001058893408123",
    nationality: "ID", //e.g.: “ID”
    domicileCountry: "ID", //e.g.: “ID”
    religion: "2",
    birthPlace: "Semarang",
    birthDate: "14081982",
    gender: "M", //“M” or “F”
    isMarried: "S",
    motherMaidenName: "Dina Maryati",
    jobCode: "01",
    education: "7",
    idType: "01",
    idNumber: "4147016201959998", //Identity Number (KTP only)
    idIssuingCity: "Jakarta Barat",
    idExpiryDate: "26102099", //ddMMyyyy
    addressStreet: "Jalan Mawar Melati",
    addressRtRwPerum: "003009Sentosa",
    addressKel: "Cengkareng Barat",
    addressKec: "Cengkareng/Jakarta Barat",
    zipCode: "11730",
    homePhone1: "0214", //Area code, e.g. 021 (3 - 4 digit) If not exist, fill with “9999”
    homePhone2: "7459", //Number after area code (min 4 digit) If not exist, fill with “99999999”
    officePhone1: "", //(Optional) Area code, e.g. 021
    officePhone2: "", //(Optional)  Number after area code
    mobilePhone1: "0812", //Operator code, e.g. 0812 (4 digit) If not exist, fill with “0899”
    mobilePhone2: "12348331", //Number after operator code (min 6 digit) If not exist, fill with “999999”
    faxNum1: "", //(Optional) Area code, e.g. 021
    faxNum2: "", //(Optional) Number after area code
    email: "agus.saputra@gmail.com",
    monthlyIncome: "8000000",
    branchOpening: "0259",
    institutionName: "PT. BNI SECURITIES",
    sid: "IDD280436215354",
    employerName: "Salman", //Employer Name / Company Name
    employerAddDet: "St Baker", //Employer street address / home street address
    employerAddCity: "Arrandelle", //Employer city address / home city address
    jobDesc: "Pedagang",
    ownedBankAccNo: "0337109074",
    idIssuingDate: "10122008",
  });
  return res;
};
```
#### Check SID
```javascript
// return as Promise of Object
const checkSID = async () => {
  const res = await rdn.checkSID({
  companyId: 'SANDBOX', // Registered participan id from KSEI
  parentCompanyId: 'STI_CHS',
  participantId: 'NI001', // Institution code, e.g.: "NI001"
  sidNumber: 'IDD1206M9527805', // SID number, e.g.: "IDD12345002"
  accountNumberOnKsei: 'NI001CRKG00146', 
  branchCode: '0259',
  ack: 'Y' // N = send data to KSEI & Y = check previous checkSID status
  });
  return res;
};
```
#### Register Investor Account
```javascript
// return as Promise of Object
const registerInvestorAccount = async () => {
  const res = await rdn.registerInvestorAccount({
    companyId: ' NI001',
    parentCompanyId: 'KSEI', // optional
    cifNumber: '9100749959',
    currency: 'IDR', // “IDR” or “USD”
    openAccountReason: '2',
    sourceOfFund: '1',
    branchId: '0259',
    bnisId: '19050813401', // Value = requestUuid.
    sre: 'NI001CX5U00109' // No. Sub rekening efek, e.g: “NI001CX5U00109”
  });
  return res;
};
```
#### Send Data Static
```javascript
// return as Promise of Object
const sendDataStatic = async () => {
  const res = await rdn.sendDataStatic({
    companyId: 'SPS App', // Registered participan id from KSEI
    parentCompanyId: 'KSEI', // optional
    participantCode: 'NI001', // Institution code, e.g: “NI001”
    participantName: 'PT. BNI SECURITIES', // Institution name, e.g.: “PT. BNI SECURITIES”
    investorName: 'SUMARNO', // Investor name
    investorCode: 'IDD250436742277', // Investor code, e.g.: "IDD250436742277"
    investorAccountNumber: 'NI001042300155', // Investor account number, e.g.: "NI001042300155"
    bankAccountNumber: '242345393', // e.g.: "242345393"
    activityDate: '20180511', // e.g: "yyyyMMdd"
    activity: 'O' // (O)pening / (C)lose / (B)lock Account / (U)nblock Account
  });
  return res;
};
```
#### Inquiry Account Info
```javascript
// return as Promise of Object
const inquiryAccountInfo = async () => {
  const res = rdn.inquiryAccountInfo({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117'
  });
  return res;
};
```
#### Inquiry Account Balance
```javascript
// return as Promise of Object
const inquiryAccountBalance = async () => {
  const res = rdn.inquiryAccountBalance({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117'
  });
  return res;
};
```
#### Inquiry Account History
```javascript
// return as Promise of Object
const inquiryAccountHistory = async () => {
  const res = rdn.inquiryAccountHistory({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117'
  });
  return res;
};
```
#### Payment Using Transfer
```javascript
// return as Promise of Object
const paymentUsingTransfer = async () => {
  const res = rdn.paymentUsingTransfer({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117',
    beneficiaryAccountNumber: '0115471119',
    currency: 'IDR', // e.g., “IDR”
    amount: '11500',
    remark: 'Test RDN' // Recommended for the reconciliation purpose
  });
  return res;
};
```
#### Inquiry Payment Status
```javascript
// return as Promise of Object
const inquiryPaymentStatus = async () => {
  const res = rdn.inquiryPaymentStatus({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    requestedUuid: 'E8C6E0027F6E429F'
  });
  return res;
};
```
#### Payment Using Clearing
```javascript
// return as Promise of Object
const paymentUsingClearing = async () => {
  const res = rdn.paymentUsingClearing({
    companyId: 'SANDBOX',
    parentCompanyId: 'STI_CHS', // optional
    accountNumber: '0115476117', // Transfer/payment provider account number
    beneficiaryAccountNumber: '3333333333', // Transfer/payment receiver account number
    beneficiaryAddress1: 'Jakarta', // Receiver address, e.g.: "Jakarta"
    beneficiaryAddress2: '', // optional
    beneficiaryBankCode: '140397',
    beneficiaryName: 'Panji Samudra', // Receiver name
    currency: 'IDR', // e.g., “IDR”
    amount: '15000', // Total payment/transfer
    remark: 'Test kliring', // //(optional)Recommended for the reconciliation purpose
    chargingType: 'OUR'
  });
  return res;
};
```
#### Payment Using RTGS
```javascript
// return as Promise of Object
const paymentUsingRTGS = async () => {
  const res = rdn.paymentUsingRTGS({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117',
    beneficiaryAccountNumber: '3333333333',
    beneficiaryAddress1: 'Jakarta',
    beneficiaryAddress2: '',
    beneficiaryBankCode: 'CENAIDJA',
    beneficiaryName: 'Panji Samudra',
    currency: 'IDR', // e.g., “IDR” 
    amount: '150000000',
    remark: 'Test rtgs', // Recommended for the reconciliation purpose
    chargingType: 'OUR'
  });
  return res;
};
```
#### Inquiry Interbank Account
```javascript
// return as Promise of Object
const inquiryInterbankAccount = async () => {
  const res = rdn.inquiryInterbankAccount({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117',
    beneficiaryBankCode: '013',
    beneficiaryAccountNumber: '01300000'
  });
  return res;
};
```
#### Payment Using Interbank
```javascript
// return as Promise of Object
const paymentUsingInterbank = async () => {
  const res = rdn.paymentUsingInterbank({
    companyId: 'NI001',
    parentCompanyId: 'KSEI', // optional
    accountNumber: '0115476117',
    beneficiaryAccountNumber: '3333333333',
    beneficiaryAccountName: 'KEN AROK', // Get from Inquiry Interbank Account
    beneficiaryBankCode: '014',
    beneficiaryBankName: 'BANK BCA', // Get from Inquiry Interbank Account
    amount: '15000'
  });
  return res;
};
```
## Get help

- [Digital Services](https://digitalservices.bni.co.id/en/)
- [API documentation](https://digitalservices.bni.co.id/documentation/public/en)
- [Stackoverflow](https://stackoverflow.com/users/19817167/bni-api-management)
- Can't find answer you looking for? email to [apisupport@bni.co.id](mailto:apisupport@bni.co.id)
