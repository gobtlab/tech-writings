# ethers.js 튜토리얼 - 1

## ethers.js란?
ethers.js는 이더리움 블록체인 및 해당 생태계와 개발자가 상호 작용하는 것에 도움을 주는 라이브러리입니다. 적은 용량(압축: 88kb, 비압축: 284kb)에도 이더리움에 필요한 모든 기능이 포함되어 있고 MIT 라이센스입니다.

## 기본 실습 환경
- npm
- node.js
- ethers.js
- Etherum Ropsten 테스트넷
- Infura Node API

## ethers.js 설치
```shell
npm install --save ethers
```

## 가져오기(import)
```js
// node.js
const { ethers } = require('ethers');

// ES6 혹은 TypeScript
// node.js에서는 package.json에 "type": "module" 값을 줘야합니다.
import { ethers } from 'ethers';
```

## Provider 설정
> Infura Node API(https://infura.io)를 사용했고 실습 대상 네트워크는 Ethereum Ropsten 테스트넷입니다.
```js
const url = 'https://ropsten.infura.io/v3/be15baa2efca4597aaaaf94c7e776973';

const provider = new ethers.providers.getDefaultProvider(url);
```

## 조회 예제(블록 번호, 가스값, 지갑 잔액)
```js
// 최신 block 번호 출력
const currentBlockNumber = await provider.getBlockNumber();
console.log(currentBlockNumber);
	
// 현재 gasPrice를 조회 후 gwei로 출력
const currentGasPrice = await provider.getGasPrice(); //  Bignumber WEI HexString
console.log(ethers.utils.formatUnits(currentGasPrice, 'gwei')); // GWEI


// 지갑주소의 ETH 잔액을 구한 후 ETH로 출력
const walletAddress = '0xba0621244CeC3874292eA876265563F8c21338Ff'; // 조회 할 지갑주소 입력
const balance = await provider.getBalance(walletAddress); // Bignumber WEI HexString
console.log(ethers.utils.formatEther(balance)); // ETH
```

