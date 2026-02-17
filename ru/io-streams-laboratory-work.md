---
layout: default
title: Лабораторная работа. Модульные программы. Ввод/вывод в C++. Библиотеки ввода/вывода
lang: ru
---

# Лабораторная работа по теме "Модульные программы. Ввод/вывод в C++. Библиотеки ввода/вывода"

## Задание для разбора

Разработайте класс ***BankAccount*** (банковский счёт) с:

- полями:

    - ФИО владельца счёта (целое число);

    - текущий баланс счёта (дробное число).

- методами:

    - внесения денег на счёт;

    - снятия денег со счёта.

Используйте перегрузки операторов ***<<*** и ***>>*** для ввода-вывода объектов через стандартные и файловые потоки. Для организации ввода/вывода данных создайте абстрактный класс и сделайте две реализации:
- одна для работы с консолью;
- другая - для файлов.

При реализации классов используйте разделение на заголовочный файл (*.hpp*) и файл с реализацией (*.cpp*).

## Разбор задания

Начнём с реализации класса BankAccount. Классы, отвечающие за реализацию бизнес логики будем располагать в папке Models. Для этого, создадим её. В дереве с файлами в правой части интерфейса Visual Studio изменим режим отображения файлов, нажав на соответствующую кнопку (Рисунок 1).

![Создание нового репозитория]({{ '/assets/images/io-streams-laboratory-work/1.png' | relative_url }}){: .center-img }
<p style="text-align: center;">Рисунок 1 – Отображение всех файлов</p>

На созданный проект нажмём правой кнопкой мыши, а затем в меню ![Кнопка Add]({{ '/assets/images/io-streams-laboratory-work/1_1.png' | relative_url }}) нажмём на ![Кнопка New Folder]({{ '/assets/images/io-streams-laboratory-work/1_2.png' | relative_url }}). В дереве файлов появится новая папка ![Папка NewFolder1]({{ '/assets/images/io-streams-laboratory-work/1_3.png' | relative_url }}), в которую можем ввести нужное название ![Папка Models]({{ '/assets/images/io-streams-laboratory-work/1_4.png' | relative_url }}).

Для создания класса нажмём на ![Кнопка Class...]({{ '/assets/images/io-streams-laboratory-work/1_5.png' | relative_url }}). В открывшемся окне введём имя для класса, .h (.hpp) и .cpp файлов (Рисунок 2).

![Создание класса]({{ '/assets/images/io-streams-laboratory-work/2.png' | relative_url }}){: .center-img }
<p style="text-align: center;">Рисунок 2 – Создание класса</p>

Сгенерированные файлы перенесём в папку *Models*. Для этого надо выделить два файла, а затем удерживая левую кнопку мыши перетащим в папку (Рисунок 3).

![Сохранение файлов класса в папке]({{ '/assets/images/io-streams-laboratory-work/3.png' | relative_url }}){: .center-img }
<p style="text-align: center;">Рисунок 3 – Сохранение файлов класса в папке</p>

В файле ***BankAccount.h*** опишем интерфейс класса, а именно его поля и методы. В данном файле приводятся только определения методов, а их реализация будет в файле .cpp. Исходя из требований к заданию, определим поля *m_owner_full_name*, *m_current_balance* и методы *cache_in*, *cache_out*.

*Объявление полей и методов*
```cpp
#pragma once

#include <string>
#include <iostream>

using namespace std;

class BankAccount
{
	private:
		string m_owner_full_name = "";
		double m_current_balance = 0;

	public:
		/// <summary>
		/// Пополенние баланса счета.
		/// </summary>
		/// <param name="delta_balance">Величина, на которую увеличится текущий баланс.</param>
		void cache_in(double delta_balance);

		/// <summary>
		/// Снятие с баланса счета.
		/// </summary>
		/// <param name="delta_banalce">Величина, на которую уменьшится текущий баланс.</param>
		void cache_out(double delta_banalce);
};
```

Помимо этого, можно определить методы для получения текущего баланса, получения и изменения ФИО владельца, а также конструктор без параметров и конструктор, принимающий ФИО владельца, начальный баланс.

*Объявление конструкторов и геттеров, сеттеров*
```cpp
public:
    BankAccount();

    BankAccount(string owner_full_name, double current_balance);

    /// <summary>
    /// GET-метод для получения ФИО владельца счета.
    /// </summary>
    /// <returns>ФИО владельца счета.</returns>
    string get_owner_full_name();

    /// <summary>
    /// SET-метод для изменения ФИО владельца счета.
    /// </summary>
    /// <param name="owner_full_name">Новое ФИО владельца счета.</param>
    void set_owner_full_name(string owner_full_name);

    /// <summary>
    /// GET-метод для получения текущего баланса счета.
    /// </summary>
    /// <returns>Текущий баланс счета.</returns>
    double get_current_balance();
```

Также переопределим операции ***<<*** и ***>>*** как дружественные функции.

*Объявление перегрузки операторов ввода, вывода*
```cpp
friend ostream& operator <<(ostream& o_stream, const BankAccount& bank_account);
friend istream& operator >>(istream& i_stream, BankAccount& bank_account);
```

Далее реализуем объявленные конструкторы и методы в .cpp файле. Начнём с реализации конструкторов. Для этого необходимо следовать шаблону:

```
<Название класса>::<Название класса>(<Аргументы конструктора>){
    <Реализация конструктора>
}
```

Реализованные конструкторы имеют следующий вид:

*Реализация конструкторов*
```cpp
BankAccount::BankAccount() {

}

BankAccount::BankAccount(string owner_full_name, double current_balance) {
	if (current_balance < 0) {
		throw invalid_argument("Параметр current_balance не должен быть отрицательным.");
	}

	m_owner_full_name = owner_full_name;
	m_current_balance = current_balance;
}
```

Для реализации методов класса необходимо следовать шаблону:

```
<Тип возвращаемого значения> <Название класса>::<Название метода>(<Аргументы метода>){
    <Реализация метода>
}
```

Реализованные GET, SET методы:

*Реализация геттеров, сеттеров*
```cpp
string BankAccount::get_owner_full_name() {
	return m_owner_full_name;
}

void BankAccount::set_owner_full_name(string owner_full_name) {
	m_owner_full_name = owner_full_name;
}

double BankAccount::get_current_balance() {
	return m_current_balance;
}
```

Реализованные основные методы:

*Реализация основных методов*
```cpp
void BankAccount::cache_in(double delta_balance) {
	if (delta_balance <= 0) {
		throw invalid_argument("Параметр delta_balance должен быть положительным.");
	}

	double new_balance = m_current_balance + delta_balance;

	m_current_balance = new_balance;
}

void BankAccount::cache_out(double delta_balance) {
	if (delta_balance <= 0) {
		throw invalid_argument("Параметр delta_balance должен быть положительным.");
	}

	double new_balance = m_current_balance - delta_balance;

	if (new_balance < 0) {
		throw logic_error("Баланс после снятия средств будет отрицательным. Операция невозможна.");
	}

	m_current_balance = new_balance;
}
```

Реализованные перегрузки операций ***<<*** и ***>>*** (т.к. они являются не методами, а внешними функциями, то им не надо указывать через **::** название класса):

*Перегрузка операторов ввода, вывода*
```cpp
ostream& operator <<(ostream& o_stream, const BankAccount& bank_account) {
	o_stream << bank_account.m_owner_full_name << "\n" << bank_account.m_current_balance << "\n";

	return o_stream;
}

istream& operator >>(istream& i_stream, BankAccount& bank_account) {
	std::getline(i_stream, bank_account.m_owner_full_name);
	i_stream >> bank_account.m_current_balance;

	return i_stream;
}
```

Для реализации классов ввода/вывода в консоль или файл создадим папку ***IOServices***, а затем создадим базовый абстрактный класс ***BankAccountIOService***, который содержит методы для ввода или вывода данных в некоторый источник. Действия по созданию аналогичны созданию папки ***Models*** и класса ***BankAccount*** (Рисунок 4).

![Создание сервиса ввода, вывода]({{ '/assets/images/io-streams-laboratory-work/4.png' | relative_url }}){: .center-img }
<p style="text-align: center;">Рисунок 4 – Создание сервиса ввода, вывода</p>

Заголовочный файл ***BankAccountIOService.h*** содержит только объявления методов. Т.к. у него нет никаких реализаций, то файл ***BankAccountIOService.cpp*** останется пустым.

*Объявление абстрактного класса*
```cpp
#pragma once
#include "../Models/BankAccount.h"

class BankAccountIOService
{
	public:
		/// <summary>
		/// Заполняет поля объекта bank_account.
		/// </summary>
		/// <param name="bank_account">Объект, который будет заполнен.</param>
		virtual void input(BankAccount& bank_account) = 0;
		
		/// <summary>
		/// Выводит поля объекта bank_accout.
		/// </summary>
		/// <param name="bank_account">Объект, который будет выведен.</param>
		virtual void output(BankAccount& bank_account) = 0;
};
```

Напишем класс ***BankAccountIOConsoleService*** для ввода/вывода в консоль. В заголовочном файле укажем базовый класс и намерение перегрузить виртуальные методы.

*Объявление реализации абстрактного класса (1)*
```cpp
#pragma once
#include "BankAccountIOService.h"

class BankAccountIOConsoleService :
    public BankAccountIOService {
    public:
        void input(BankAccount& bank_account) override;
        void output(BankAccount& bank_account) override;
};
```

В файле .cpp уже описываем реализацию методов, соблюдая шаблон реализации методов. Модификатор override при этом уже указывать не требуется.

*Реализация абстрактного класса (1)*
```cpp
#pragma once
#include "BankAccountIOConsoleService.h"
#include <iostream>

using namespace std;

void BankAccountIOConsoleService::input(BankAccount& bank_account) {
	cin >> bank_account;
}

void BankAccountIOConsoleService::output(BankAccount& bank_account) {
	cout << bank_account;
}
```

Аналогичным образом реализуем класс ***BankAccountIOFileService*** для ввода/вывода в файл. Помимо методов базового класса определим также конструктор для того, чтобы можно было изменять папку для сохранения файлов.

*Объявление реализации абстрактного класса (2)*
```cpp
#pragma once
#include "BankAccountIOService.h"
#include <string>

using namespace std;

class BankAccountIOFileService :
    public BankAccountIOService {
    private:
        string m_base_folder;

    public:
        BankAccountIOFileService(string base_folder);

        /// <summary>
        /// Заполняет объект bank_account на основе файла base_folder/<ФИО клиента>.
        /// </summary>
        /// <param name="bank_account">Объект для заполнения.</param>
        void input(BankAccount& bank_account) override;
        
        /// <summary>
        /// Заполняет файл base_folder/<ФИО клиента> на основе объекта bank_account.
        /// </summary>
        /// <param name="bank_account">Объект, на основе которого создается файл.</param>
        void output(BankAccount& bank_account) override;
};
```

Конструктор и метод заполнения имеют следующую реализацию:

*Реализация абстрактного класса (2)*
```cpp
BankAccountIOFileService::BankAccountIOFileService(string base_folder) {
	m_base_folder = base_folder;
}

void BankAccountIOFileService::input(BankAccount& bank_account) {
	string owner_full_name = bank_account.get_owner_full_name();
	string file_path = m_base_folder + "/" + owner_full_name;

	ifstream if_stream(file_path);

	if (!if_stream.is_open()) {
		throw logic_error("Файл " + file_path + " не получилось открыть.");
	}

	if_stream >> bank_account;

	if_stream.close();
}
```

Метод сохранения объекта в файл:

*Реализация метода сохранения объекта в файл*
```cpp
void BankAccountIOFileService::output(BankAccount& bank_account) {
	string owner_full_name = bank_account.get_owner_full_name();
	string file_path = m_base_folder + "/" + owner_full_name;

	ofstream of_stream(file_path);

	if (!of_stream.is_open()) {
		throw logic_error("Файл " + file_path + " не получилось открыть.");
	}

	of_stream << bank_account;

	of_stream.close();
}
```

Последним шагом необходимо написать код основной функции ***main***. Определять способ заполнения и вывода объекта можно на основе пользовательского ввода.

*Реализация функции main*
```cpp
#pragma once
#include <iostream>
#include "Models/BankAccount.h"
#include "IOServices/BankAccountIOService.h"
#include "IOServices/BankAccountIOFileService.h"
#include "IOServices/BankAccountIOConsoleService.h"
#include <string>

using namespace std;

int main()
{
    try {
        bool is_work_with_file;
        cout
            << "Выберите способ для работы с объектом:" << "\n"
            << "0 - для работы через консоль;" << "\n"
            << "любое другое число - для работы через файл;" << "\n";
        cin >> is_work_with_file;

        BankAccountIOFileService bank_account_io_file_service = BankAccountIOFileService(".");
        BankAccountIOConsoleService bank_account_io_console_service = BankAccountIOConsoleService();

        BankAccountIOService& io_service = is_work_with_file ?
            static_cast<BankAccountIOService&>(bank_account_io_file_service) :
            static_cast<BankAccountIOService&>(bank_account_io_console_service);

        BankAccount& bank_account_origin = *(new BankAccount("Иванов Иван Иванович", 100));

        io_service.output(bank_account_origin);

        BankAccount& bank_account_readed = *(new BankAccount());

        io_service.input(bank_account_readed);

        cout << bank_account_readed << endl;
    }
    catch (exception ex) {
        cerr << ex.what() << endl;
    }
}

```

Полный текст программы приведён на GitHub по [ссылке](https://github.com/maryshev3/BankAccountApplication/tree/main).

## Задание для самостоятельного решения

Разработайте класс InventoryItem для управления складскими запасами определённого товара с:

- полями:

    - код товара (строка);

    - описание товара (строка);

    - количество товара на складе (целое число);

    - цена за единицу товара в рублях (дробное число).

- методами:

    - увеличения количества товара на складе;

    - списания определённого количества товара на складе;

    - подсчёта общей цены товара на складе.

Используйте перегрузки операторов ***<<*** и ***>>*** для ввода-вывода объектов через стандартные и файловые потоки. Для организации ввода/вывода данных создайте абстрактный класс и сделайте две реализации:

- одна для работы с консолью

- другая - для файлов.