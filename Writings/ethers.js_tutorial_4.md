# ethers.js 튜토리얼 - 4

## 내용
Wallet 클래스의 함수로 지갑을 생성합니다.
생성한 객체에서 지갑주소, 개인키, mnemonic을 확인합니다.

### Wallet 인스턴스 생성 후 지갑주소, mnemonic, 개인키 출력
```js
const { ethers } = require('ethers');

// Wallet 인스턴스를 생성 후 개인키, 니모닉, 지갑주소 출력
const wallet = ethers.Wallet.createRandom();
console.log(wallet.address); // 지갑주소
console.log(wallet.mnemonic); // mnemonic phrase
console.log(wallet._signingKey().privateKey); // 개인키
```

### mnemonic(12단어)으로 Wallet 인스턴스 생성 후 지갑주소, 개인키 출력
```js
const mnemonic = 'quiz element impose gap avoid today card legend hedgehog any february mask';
const mWallet = ethers.Wallet.fromMnemonic(mnemonic);
console.log(wallet.address); // 지갑 주소
console.log(mWallet.privateKey); // 개인키
```
