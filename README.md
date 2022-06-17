# **GnoLand** node and test
**Requirements | Требования:** 

1CPU/2Gb RAM (на утюге заведется)
## Подготовка и установка
 **Update all | Апдейтим все**

	sudo apt update && sudo apt upgrade -y
**Install dependences | Ставим зависимости**

	sudo apt install make clang pkg-config libssl-dev libclang-dev build-essential git curl ntp jq llvm tmux htop screen -y

**Install Go | Ставим GO**

	ver="1.18.3"
	cd $HOME
	wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
	sudo rm -rf /usr/local/go
	sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
	rm "go$ver.linux-amd64.tar.gz"
	echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
	source ~/.bash_profile
	go version

**Install Node | Ставим ноду**

	git clone https://github.com/gnolang/gno/
	cd gno
	make
**Generate mnemonic | генерируем кей**

	./build/gnokey generate
Save it! | Сохраните сид !

**Add account | добавляем акк**
create password and input seed | создайте пароль и введите сид

	./build/gnokey add account --recover
save ur address | сохраните адрес

## Operations with account | Операции с аккаунтом

### Let's get some tokens | намутим немного токенов
We'll need 2200gnot and exist 3 ways to get it | нужны 2200gnot и есть 3 пути получить их:

1. **Script | Скрипт** 
change < ADDRESS > to ur address | замените < ADDRESS > вместе со скобками на свой адрес:

		while true; do curl 'https://gno.land:5050/' --data-raw 'toaddr=<ADDRESS>'; ./build/gnokey query "bank/balances/<ADDRESS>" --remote gno.land:36657; sleep 2; done
2. **[Faucet](https://gno.land/faucet)**
3. **[Discord](https://discord.gg/DDC6akkQnT)**

**Register Account | Регистрируем аккаунт**
change < ADDRESS > to ur address | замените < ADDRESS > вместе со скобками на свой адрес:

	./build/gnokey query auth/accounts/<ADDRESS> --remote gno.land:36657

Save *“account_number”* and *“sequence”* | Сохраняем *“account_number”* и *“sequence”*

**Create file with information about ur node | Создаем файл с инфой о вашей ноде**

	./build/gnokey maketx call <ADDRESS> --pkgpath "gno.land/r/users" --func "Register" --gas-fee 1gnot --gas-wanted 3000000 --send "2000gnot" --args "" --args "<USERNAME>" --args "" > unsigned.tx
Change < ADDRESS > and < USERNAME > on urs (only small letters) | Замените < ADDRESS > и < USERNAME > на ваши (только маленькие символы 6-17):
**Create transaction | Создаем транзакцию**

	./build/gnokey sign <ADDRESS> --txpath unsigned.tx --chainid testchain --number <ACCOUNTNUMBER> --sequence <SEQUENCENUMBER>  > signed.tx
Change data in angle brackets and delete brackets here and everywhere in future | Замените данные в треугольных скобках и удалите сами скобки (тут и далее)

**Make transaction | Проводим транзакцию**

	./build/gnokey broadcast signed.tx --remote gno.land:36657

*Our USERNAME must appear [here](https://gno.land/r/users) | Наш USERNAME должен появиться [тут](https://gno.land/r/users)*

**We have to write post somewhere and save URL | Нам нужно сделать пост о проекте и сохранить URL**
Now we have to post tx with this URL | теперь нам нужно запостить транзу с URL

	./build/gnokey maketx call <ADDRESS> --pkgpath "gno.land/r/boards" --func "CreateReply" --gas-fee 1gnot --gas-wanted 3000000 --send "" --broadcast true --chainid testchain --args "1" --args "8" --args "8" --args "<URL>" --remote gno.land:36657

**U can check result [here](https://gno.land/r/boards:gnolang/8) | Результат можете проверить [тут](https://gno.land/r/boards:gnolang/8)** 
