# Frequently Asked Questions

Este documento irá focar-se na tecnologia por trás da plataforma da Nebulas. Para perguntas mais gerais, por favor veja o [FAQ do Reddit](https://www.reddit.com/r/nebulas/comments/7nt5y0/frequently_asked_questionsfaq/).

Para uma compreensão melhor da plataforma da Nebulas, o [Livro Branco Técnico da Nebulas](https://nebulas.io/docs/NebulasTechnicalWhitepaper.pdf) é recomendado.

Índice

1. [Nebulas Rank \(NR\)](faq.md#nebulas-rank-nr)
2. [Nebulas Force \(NF\)](faq.md#nebulas-force-nf)
3. [Developer Incentive Protocol \(DIP\)](faq.md#developer-incentive-protocol-dip)
4. [Proof of Devotion \(PoD\) Consensus Algorithm](faq.md#proof-of-devotion-pod-consensus-algorithm)
5. [Nebulas Search Engine](faq.md#nebulas-search-engine)
6. [Fundamentals](faq.md#fundamentals)
   1. [Nebulas Name Service \(NNS\)](faq.md#nebulas-name-service-nns)
   2. [Lightning Network](faq.md#lightning-network)
   3. [Nebulas Token \(NAS\)](faq.md#nebulas-token-nas)
   4. [Smart Contracts](faq.md#smart-contracts)
      1. [Language Support](faq.md#what-languages-will-be-supported-when-main-net-launches)
      2. [Ethereum Compatibility](faq.md#will-ethereum-smart-contracts-solidity-be-fully-supported)

## Nebulas Rank \(NR\)

Mede o valor ao considerar liquidez e propagação de um endereço. O Nebulas Ranking tenta estabelecer uma abordagem fidedígna, computacional, e determinística. Com o sistema de classificação de valor, iremos ver mais dApps excepcionais na plataforma da Nebulas.

#### When will Nebulas Rank \(NR\) be ready?

> O Nebulas Rank foi lançado em Dezembro de 2018. No momento da escrita deste artigo, o servidor NR Query está offline, desde a actualização do algoritmo. Pode contribuir para o projecto de refatoração do código [aqui](https://go.nebulas.io/project/130).

#### Will dApps with more transactions naturally be ranked higher?

> Não necessariamente, visto que o número de transacções apenas aumenta o grau de in-and-out durante um período de tempo, até um valor predeterminado. A maneira em que o Nebulas Rank é calculado usa, entre várias outras variaveis, a median account stake. A median account stake é o balanço mediano de um endereço durante um certo período de tempo.

#### How does the Nebulas Rank \(NR\) separate quality dApps from highly transacted dApps?

> Ao utilizar o Median Account Stake para calcular o NR, o Nebulas Rank garante justiça e resiste manipulação a um grau elevado, garantindo a probabilidade de dApps de alta qualidade flutuarem para o topo da hierarquia.

#### Is the Nebulas Ranking algorithm open-source?

> Sim.

#### Who can contribute to the algorithm?

> De momento, a equipa da Nebulas está responsável pelo desenvolvimento do algoritmo. No entanto, todos podem fazer sugestões, relatórios de bugs, e contribuições de código. O repositório do SDK encontra-se [aqui](https://github.com/nebulasio/nebnr), e o do Nebulas Rank Offline Service [aqui](https://github.com/nebulasio/nr-service).

#### Can the Nebulas Rank \(NR\) algorithm be cheated?

> Nada é invulnerável, mas o objectivo é tornar a manipulação do algoritmo bastante cara e o mais difícil possível.

## Nebulas Force \(NF\)

Suporte a actualização dos protocolos centrais e smart contracts nas blockchains. Confere a habilidade de auto-evolução ao sístema da Nebulas e às suas aplicações. Com o Nebulas Force, desenvolvedores podem criar várias iterações de aplicações complexas, e essas mesmas podem-se adaptar dinamicamente à comunidade ou variações do mercado.

#### When will Nebulas Force \(NF\) be ready?

> De acordo com o [roadmap](https://wiki.nebulas.io/en/latest/roadmap.html) o Nebulas Force será lançado no quarto trimestre de 2019.

#### Can smart contracts be upgraded?

> Yes, \[short summary explaining how it works\]

#### How is Nebulas Force \(NF\) smart contract upgrading better than other solutions that are currently or soon-to-be available?

> answer here

#### Can the Nebulas blockchain protocol code be upgraded without forking?

> Yes, \[short summary explaining how it works\]

#### Can the Nebulas Virtual Machine \(NVM\) be upgraded?

> Yes, \[short summary explaining how it works\]

## Developer Incentive Protocol \(DIP\)

Designed to build the blockchain ecosystem in a better way. The Nebulas token incentives will help top developers to create more values in Nebulas.

#### When will the Developer Incentive Protocol \(DIP\) be ready?

> answer here

#### Will there be a limit as to how many rewards one dApp can receive?

> answer here

#### Will developers still be able to do their own ICOs?

> answer here

#### Will only the top Nebulas Rank \(NR\) dApps receive rewards?

> answer here

#### How often will rewards be given?

> answer here

#### How will you stop cheaters?

> The way the DIP is is designed makes it very hard for cheaters to be successful. Since smart contracts can only be called passively, it would be highly cost ineffective for a user to try to cheat the system. More about this topic can be read in the Technical Whitepaper.

## Proof of Devotion \(PoD\) Consensus Algorithm

To build a healthy ecosystem, Nebulas proposes three key points for consensus algorithm: speediness, irreversibility and fairness. By adopting the advantages of PoS and PoI, and leveraging NR, PoD will take the lead in consensus algorithms.

#### When will the Proof of Devotion \(PoD\) Consensus Algorithm be ready?

> answer here

#### What consensus algorithm will be used until PoD is ready?

> answer here

#### How are bookkeepers chosen?

> The PoD consensus algorithm uses the Nebulas Rank \(NR\) to qualify nodes to be eligible. One node from the set is randomly chosen to propose the new block and the rest will become the validators.

#### Do bookkeepers still have to stake?

> Yes, once chosen to be a validator for a new block, the validator will need to place a deposit to continue.

#### How many validators will there be in each set?

> answer here

#### What anti-cheating mechanisms are there?

> answer here

## Nebulas Search Engine

Nebulas constructs a search engine for decentralized applications based on Nebulas value ranking. Using this engine, users can easily find desired decentralized applications from the massive market.

#### When will the Nebulas Search Engine be ready?

> answer here

#### Will you be able to search dApps not on the Nebulas platform?

> answer here

#### Will the Nebulas Search Engine also be decentralized?

> answer here

#### Will the Nebulas Rank \(NR\) control the search results ranking?

> answer here

#### What data will you be able to search?

> We plan many different ways to be able to search the blockchain:
>
> * crawl relevant webpages and establish mapping between them and the smart contracts
> * analyze the code of open-source smart contracts
> * establish contract standards that enable easier searching

## Fundamentals

### Nebulas Name Service \(NNS\)

By using smart contracts, the Nebulas development team will implement a DNS-like domain system named Nebulas Name Service \(NNS\) on the chain while ensuring that it is unrestricted, free and open. Any third-party developers can implement their own domain name resolution services independently or based on NNS.

#### When will the Nebulas Name Service be ready?

> answer here

#### When a name is bid on, how long do others have to place their bid?

> answer here

#### How do others get notified that a name is being bid on?

> answer here

#### When a name is reserved who gets the bid amount?

> answer here

#### If I want to renew my name after one year will I need to deposit more NAS?

> answer here

#### Will we be able to reserve names prior to the launch of NNS?

> answer here

### Lightning Network

Nebulas implements the lightning network as the infrastructure of blockchains and offers flexible design. Any third-party developers can use the basic service of lightning network to develop applications for frequent transaction scenarios on Nebulas. In addition, Nebulas will launch the world’s first wallet app that supports the lightning network.

#### When will lightning network be supported?

> answer here

### The Nebulas Token \(NAS\)

The Nebulas network has its own built-in token, NAS. NAS plays two roles in the network. First, as the original money in the network, NAS provides asset liquidity among users, and functions as the incentive token for PoD bookkeepers and DIP. Second, NAS will be charged as the calculation fee for running smart contracts. The minimum unit of NAS is 10−18 NAS.

#### What will happen to the Nebulas ERC20 tokens when NAS is launched?

> answer here

#### Will dApps on the Nebulas platform be able to issue their owns ICOs and tokens?

> answer here

### Smart Contracts

#### What languages will be supported when Main-net launches?

> answer here

#### Will Ethereum Smart Contracts \(Solidity\) be fully supported?

> answer here

#### What other language support will follow \(and when\)?

> answer here

#### binary storage

What is recommended way to store binary data in Nebulas blockchain? Is it possible at all? Do you encourage such use of blockchain? Also, i couldn't find information regarding GlobalContractStorage mentioned in docs, what is it?

> Currently binary data can be stored on chain by binary transaction. The limit size of binary is 128k. But we don’t encourage storing data on the chain because the user might store some illegal data.
>
> `GlobalContractStorage`not currently implemented. It provides support for multiple contract sharing data for the same developer.

#### ChainID & connect

Can you tell us what the chainID of Mainnet and Testnet is? I have compiled the source code of our nebulas, but not even our test network?

> chainID of Nebulas:

* Mainnet: 1
* Testnet: 1001
* private: default 100, users can customize the values.

> The network connection:

* Mainnet:
  * source code:[master](https://github.com/nebulasio/go-nebulas/tree/master)
  * wiki:[Mainnet](https://github.com/nebulasio/wiki/blob/master/mainnet.md)
* Testnet:
  * source code:[testnet](https://github.com/nebulasio/go-nebulas/tree/testnet)
  * wiki:[Testnet](https://github.com/nebulasio/wiki/blob/master/testnet.md)

#### smart contract deploy

Our smart contract deployment, I think is to submit all contract code directly, is the deployment method like this?

> Yeah, We can deploy the contract code directly, just as it is to release code to the NPM repository, which is very simple and convenient.

#### smart contract IDE

We don't have any other smart contract ides, like solidity's "Remix"? Or is there documentation detailing which contract parameters can be obtained? \(because I need to implement the random number and realize the logic, I calculate the final random number according to the parameters of the network, so I may need some additional network parameters that will not be manipulated.\)

> You can use [web-wallet](https://github.com/nebulasio/web-wallet) to deploy the contract, it has test function to check the parameters and contract execution result.

