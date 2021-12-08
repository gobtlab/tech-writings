# ethers.js 튜토리얼 - 3

## Token 전송 예제
ERC-20 Token 전송 예제입니다.

### 사용 순서
1. 보내는 사람 지갑의 개인키(privateKey)를 입력
2. 함수호출에 사용할 매개변수 입력
 - 전송할 ERC-20 Token의 컨트랙트 주소(contractAddress)
 - 전송 받을 지갑주소(to)
 - 전송할 ERC-20 Token 수량(amount)
3. sendToken() 함수에 설정한 매개변수를 넣고 호출
4. 전송이 완료되었다면 상세내역(receipt)이 출력됨


### send() 함수 설명
1. 네트워크 노드 URL로 provider 객체 생성
2. 개인키로 signer 객체를 생성
3. ERC-20 Token 컨트랙트 주소, ABI, Signer 객체를 매개변수로 지정 후 Contract 객체 생성
4. 전송 option 설정
5. ABI의 transfer() 함수 호출
6. 전송 완료 시 상세내역(receipt)을 리턴받음


```js
const { ethers } = require('ethers');

// 해당 URL은 필자가 개별로 Infura에서 생성한 프로젝트 URL입니다.
// https://infura.io 에서 가입 후 직접 프로젝트를 생성해 개인 설정 하시는 것을 추천합니다.
const url = 'https://ropsten.infura.io/v3/be15baa2efca4597aaaaf94c7e776973'; // Ethereum Ropsten testnet

const privateKey = '보내는 주체의 지갑 개인키 입력';
const contractAddress = '전송할 ERC-20 Token의 컨트랙트 주소 입력'; // Token 컨트랙트 주소
const to = '전송받을 지갑 주소 입력';
const amount = '전송할 ERC-20 Token 수량 입력';

const abi = [
	// Read-Only Functions
	"function decimals() view returns (uint8)",

	// Authenticated Functions
	"function transfer(address to, uint amount) returns (boolean)",

	// Events
	"event Transfer(address indexed from, address indexed to, uint amount)"
];


// Token 전송 함수 호출
// 전송 성공 시 전송 결과(receipt)가 출력됩니다.
sendToken(contractAddress, to, amount).then(console.log);


/**
 * @param {string} contractAddress 전송할 ERC-20 Token 컨트랙트 주소
 * @param {string} to 전송 받을 지갑주소
 * @param {string} amount 전송 할 ERC-20 Token 수량
 */
async function sendToken(contractAddress, to, amount) {
	try {
		const provider = new ethers.providers.getDefaultProvider(url);
		const signer = new ethers.Wallet(privateKey).connect(provider);
		const contract = new ethers.Contract(contractAddress, abi, signer);
		const decimals = await contract.decimals();
	
		const options = {
			gasLimit: ethers.utils.hexlify(100000), // 가스 한도 설정(사용되지 않는 가스는 반환됨)
			gasPrice : await provider.getGasPrice() // gasPrice 추측값을 설정
		};
	
		const receipt = await contract.transfer(to, ethers.utils.parseUnits(amount, decimals), options);
		return receipt;

	} catch (error) {
		console.error(error);
	}
}


```

## 전송 결과값 예시
```js
{
  type: 2,
  chainId: 3,
  nonce: 389,
  maxPriorityFeePerGas: BigNumber { _hex: '0x086e33bff9', _isBigNumber: true },
  maxFeePerGas: BigNumber { _hex: '0x086e33bff9', _isBigNumber: true },
  gasPrice: null,
  gasLimit: BigNumber { _hex: '0x0186a0', _isBigNumber: true },
  to: '0xb64faF713A4A0A895b6dcF442795F0F5C6A64E63',
  value: BigNumber { _hex: '0x00', _isBigNumber: true },
  data: '0xa9059cbb00000000000000000000000056092d54c6c0c7b7d1c126a4075a3d664f3c482c00000000000000000000000000000000000000000000001043561a8829300000',
  accessList: [],
  hash: '0xe84cc42ef23114f77e185fb52e25f5744d08bd0c85704d66d99244eec948ec18',
  v: 1,
  r: '0xd2a9ecf873bb3a9df5edc5cb069124dc7dc2c370499a174c2fbbeb807d29b601',
  s: '0x4ccb226d8acc5aaaf6e54fbe1a743793ef94c081581baf63ce9cf4f92b12ede2',
  from: '0xba0621244CeC3874292eA876265563F8c21338Ff',
  confirmations: 0,
  wait: [Function (anonymous)]
}
```