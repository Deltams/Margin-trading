# Margin-trading

Margin Trading - это продукт, в котором пользователю (трейдеру), предоставляется заемный капитал для покупки активов (например, WETH).

### Целевая аудитория
1. Trader: торгует используя заемный капитал. Покупает токены дешево, продает дорого (спекулянт).
2. Liquidity Provider: предоставляет заемный капитал для трейдеров. За предоставление капитала взимает с трейдеров (процентную ставку). Рассматриваем это как “банковский вклад”.

### Ключевая ценность продукта
* Снижение требований по капиталу: трейдеры могут совершать торговые операции в 10x превышающих от их собственных средств, без предоставления каких-либо данных о себе (e-mail, номер телефона, ФИО и т. д.).
* Инвесторы зарабатывают предсказуемую доходность **без рисков*** к своему капиталу *(**безопастность обеспечивается правилами смарт-контракта**).

### Пример
Главная задача продукта, дать возможность торговать трейдеру на заемный капитал, и одновременно с этим обеспечить безопастность капитала Liquidity Provider’a.

Для трейдера в этом типе продукта есть два исхода: 
1. 🤑 Заработал деньги: возвращает заемный капитал, получает обратно свой залог + заработанную прибыль.
2. 😖 Потерял деньги: возвращает заемный капитал, получает остаток своего капитала, либо не получает ничего, т.к потерял все на трейдинге.
(Алиса далее играет роль трейдера)

#### Алиса заработала деньги
1. Алиса создает маржинальный аккаунт (Trader account) и делает депозит размером в $100.
2. Платформа предоставляет займ в x10 раз от внесенной суммы (100 * 10 = 1,000).
3. Теперь Алиса может купить ERC20 токен используя $1,000 (заемный капитал, который выдан по ставке 2%).
4. Алиса использовала $1,000 заёмного капитала и купила 1 WETH (цена на момент покупки 1 WETH = $1,000).
5. Спустя несколько дней, Алиса проверяет свой счет и видит, что WETH стоит $1,200 (на 20% выше, чем первоначальная цена покупки).
6. Алиса продает 1 WETH стоимостью $1,200 и теперь имеет на своем аккаунте $1,300 (долг составляет $1,000 + $20 (проценты) = $1020).
7. Алиса возвращает $1,020 долга и выводит $280 ($100 - начальный капитал Алисы, который был залогом, и еще $180 - это прибыль от проданного WETH).
В этом варианте, Алиса заработала +$180 при изначально вложенных $100 и Liquidity Provider остался при своем капитале +$20.

#### Алиса понесла убытки
1. Алиса создает маржинальный аккаунт (Trader account) и делает депозит размером в $100.
2. Платформа предоставляет займ в x10 раз от внесенной суммы (100 * 10 = 1,000).
3. Теперь Алиса может купить ERC20 токен используя $1,000 (заемный капитал).
4. Спустя несколько дней WETH падает в цене и теперь стоит $910 (на 9% ниже, чем первоначальная цена покупки).
5. Стоимость аккаунта Алисы составляет $1,010 ($910 — стоимость купленного ранее WETH, и $100 это изначальный капитал Алисы).
6. Для защиты капитала Liquidity Provider’a система принудительно закрывает позиции трейдера и возвращает в систему $1,010 ($1,000 — размер долга, $10 — штраф за ликвидацию).
В этом варианте, Алиса потеряла $100 (-100% капитала), и Liquidity Provider остался при своем капитале +$10.

## Развертывание и подключение контрактов в тестовой сети HardHat
(на компьютере, где разворачивается проект, по умолчанию уже установлен [Node.js](https://nodejs.org/en) и [MetaMask](https://metamask.io/) созданный от ключевых слов: 1. test, 2. test, 3. test, 4. test, 5. test, 6. test, 7. test, 8. test, 9. test, 10. test, 11. test, 12. junk)
1. Скопируйте себе проект на компьютер в папку.
2. Запустите консоль в папке, где находится проект.
3. В командной строке введите ```npm i ```. (Скачиваются дополнительные пакеты, процесс может занять некоторое время)
4. Чтоб убедиться, что все хорошо установилось введите в консоль ```npx hardhat compile```.
5. Введите в консоль команду ```npx hardhat node```. (Данная команда локально развернет тестовую сеть. **Внимание** При каждом новом запуске ```npx hardhat node``` необходимо чистить данные активности от MetaMask. (Настройки -> Дополнительно -> Очистить данные вкладки активности))
6. Откройте новый терминал (консоль) и введите следующую команду ```npx hardhat run --network localhost scripts/deploy.js```. (Данная команда произведет загрузку контрактов в нужном порядке и устанавливает необходимые зависимости между контрактами в тестовой сети)
7. Откройте MetaMask и зайдите в Настройки -> Сети -> Добавить сеть -> Добавить сеть вручную.
8. Имя сети введите удобное для Вас, например *My local host*, URL RPC можно найти в консоли, где была прописана команда ```npx hardhat node``` или ввести значение по умолчанию *http://127.0.0.1:8545/* или *http://localhost:8545*, ID блокчейна указываем *1337* (данный id можно найти в файле **hardhat.config.js**), Cимвол валюты *ETH*.
9. Переключаемся на тестовую сеть *My local host*.
10. Для взаимодействия frontend'а с контрактами использовался провайдер MetaMask.
    
Более подробную информацию можно найти по следующим ссылкам:
[Run a development network](https://docs.metamask.io/wallet/how-to/get-started-building/run-devnet/)
[Reference HardHat](https://hardhat.org/hardhat-network/docs/reference)

## Контракты, параметры и функций

Ознакомиться с моделью взаимодействия контрактов можно через [Графический сервис](https://www.drawio.com/) загрузив файл **MarginTradingDiagram.drawio**.

### LiquidityPool

#### Глобальные переменные

**IERC20 public USDC;**
интерфейс для взаимодействия с ERC20 токенами, в данном случае с USDC

**uint256 public balanceX;**
всего внесенных денег от инвесторов, расчет идет в USDC

**uint256 public balanceY;**
всего выпущенных виртуальных токенов, нужно для расчета индивидуальной доли прибыли каждого инвестора

**ICentralAccount public ICA;**
интерфейс для взаимодействия с центральным аккаунтом

**using TransferHelper for IERC20;**
контракт для безопасного перевода токенов ERC20 между контрактами

**uint256 constant USDC_DECIMALS = 10 ** 6;**
константа, показывает количество знаков после запятой, нужна для расчета USDC

**uint256 constant SHARE_DECIMALS = 10 ** 18;**
константа, показывает количество знаков после запятой, нужна для расчета общей доли

**mapping(address => uint256) investorToShare;**
доля прибыли Liquidity Provider’a от всех внесенных денег

#### Функции

**constructor(address _USDC, address _CA, uint256 _amount)**
начальное создание контракта (функция, которая вызывается при deploy контракта)

*address _USDC* - адрес контракта ERC20 токена в основной/тестовой сети (в данном случае USDC)
*address _CA* - адрес центрального аккаунта в основной/тестовой сети
*uint256 _amount* - начальный капитал для контракта Liquidity Pool (Желательно указать как 100 USDC и перевести на центральный аккаунт данную сумму)

**function transferToLP(uint256 _amount) external**
переводит денеги в Liquidity pool (все деньги хранятся на Central account)

*uint256 _amount* - количество денег для перевода (указывать в виде USDC)

**function accrueProfit(uint256 _amount) external**
начисляет полученную прибыть инвесторам (вкладчикам)

*uint256 _amount* - полученная прибыль (подавать в виде USDC)

**function accrueLoss(uint256 _amount) external**
начисляет полученный убыток инвесторам (вкладчикам)

*uint256 _amount* - полученный убыток (подавать в виде USDC)

**function transfer(address _from, address _to, uint256 _amount) internal**
переводит USDC от владельца к определенному адресу (получателю)

*address _from* - адрес владельца USDC
*address _to* - адрес получателя USDC
*uint256 _amount* - количество передаваемых USDC

**function safeTransferFrom(address _token, address _from, address _to, uint256 _amount) internal**
безопасный перевод ERC20 токенов от владельца к получателю

*address _token* - адрес отправляемого токенов ERC20 (в нашем случае USDC)
*address _from* - адрес владельца токенов ERC20 (в нашем случае USDC)
*address _to* - адрес получателя токенов ERC20 (в нашем случае USDC)
*uint256 _amount* - количество передаваемых токенов ERC20 (в нашем случае USDC)

**function withdraw(uint256 _amount) external**
выводит USDC из пула ликвидности на адрес отправителя транзакции

*uint256 _amount* - количество выводимых USDC из пула ликвидности

**function getUserBalance() public view returns (uint256)**
показывает текущий баланс поставщика ликвидности (вкладчика)

### CentralAccount

#### Глобальные переменные

**address SC;**

**IERC20 public USDC;**

**IERC20 public WETH;**

**ILiquidityPool public ILP;**

**ITraderAccount public ITRA;**

**uint256 public countUSDCOwner;**

**uint256 public countUSDCTraders;**

**using TransferHelper for IERC20;**
контракт для безопасного перевода токенов ERC20 между контрактами

**uint16 public ownerProfit = 1000;**

**uint16 constant COEF_OWNER_PROFIT = 10000;**

#### Функции

### RiskManager

#### Глобальные переменные

#### Функции

### SwapContract

#### Глобальные переменные

#### Функции

### TraderAccount

#### Глобальные переменные

#### Функции
