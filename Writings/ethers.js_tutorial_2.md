# ethers.js 튜토리얼 - 2

## 전송 예제
이더리움 테스트넷 네트워크에서 ETH를 전송하는 간단 예제입니다.

### 사용방법
1. privateKey 변수에 보내는 사람 지갑의 개인키를 입력
2. 전송하는 사용자 지갑주소(from), 전송받는 지갑주소(to), 전송할 수량(amount) 입력
3. 작성한 send함수 매개변수에 위에서 입력한 변수를 넣고 호출
4. 전송 성공 시 전송 결과(receipt)를 출력


### send() 함수 설명
1. 네트워크 노드 URL로 provider 객체 생성
2. 개인키로 signer 객체를 생성
3. 전송에 필요한 트랜잭션(tx) 매개변수 설정
4. signer 객체의 sendTransaction 함수에 매개변수 설정 후 호출
5. 전송 성공 시 전송 결과(receipt)를 리턴받음


```js
const { ethers } = require('ethers');

const privateKey = 'Your private key';
const url = 'https://ropsten.infura.io/v3/be15baa2efca4597aaaaf94c7e776973'; // Ethereum Ropsten testnet
// 해당 URL은 필자가 개별로 Infura에서 생성한 프로젝트 URL입니다.
// https://infura.io 에서 가입 후 직접 프로젝트를 생성해 개인 설정 하시는 것을 추천합니다.

let from = '전송하는 사용자의 지갑 주소 입력';
let to = '전송받는 지갑 주소 입력';
let amount = '전송할 수량 입력'; // i.e) 0.1, 0.03, 1 ...

// 전송 함수 호출 후 전송 결과 값 호출
send(from, to, amount).then(console.log);


/**
 * @param {string} from ETH를 전송하는 지갑주소
 * @param {string} to ETH를 전송받을 지갑주소
 * @param {string} amount 전송할 ETH 수량
 */
async function send(from, to, amount) {
	try {
		const provider = new ethers.providers.getDefaultProvider(url);
		const signer = new ethers.Wallet(privateKey).connect(provider);

		const tx = {
			from : from,
			to : to,
			value : ethers.utils.parseEther(amount),
			gasLimit : ethers.utils.hexlify(100000), // 가스 한도 설정(사용되지 않는 가스는 반환됨)
			gasPrice : await provider.getGasPrice() // gasPrice 추측값을 설정
		}

		let result = await signer.sendTransaction(tx);
		return result;
	} catch (e) {
		console.error(e);
	}
}
```


## 전송 결과값 예시
```json
{
  type: 2,
  chainId: 3,
  nonce: 388,
  maxPriorityFeePerGas: BigNumber { _hex: '0x59682f08', _isBigNumber: true },
  maxFeePerGas: BigNumber { _hex: '0x59682f08', _isBigNumber: true },
  gasPrice: null,
  gasLimit: BigNumber { _hex: '0x0186a0', _isBigNumber: true },
  to: '0x56092d54c6C0C7B7d1c126A4075A3D664f3c482c',
  value: BigNumber { _hex: '0x470de4df820000', _isBigNumber: true },
  data: '0x',
  accessList: [],
  hash: '0x9558c13d20f77646992b181bcd39b73e7f1cbf268d185749cd3a5dd914b7abe2',
  v: 1,
  r: '0x9be13b17749e4e12827f502e68270ee87c753404a5c54cfa3da6fe404c00f899',
  s: '0x0aa0d81987398fcb3c3574d5cf3a2a05187337c545a0fd8231d9f20f3b66580b',
  from: '0xba0621244CeC3874292eA876265563F8c21338Ff',
  confirmations: 0,
  wait: [Function (anonymous)]
}
```


