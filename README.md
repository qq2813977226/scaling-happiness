#include <iostream>
#include <vector>
#include <string>

using namespace std;
// 图书类
class Book {
private:
    string title;
    string author;
    bool isElectronic;

public:
    Book(string _title, string _author, bool _isElectronic) : title(_title), author(_author), isElectronic(_isElectronic) {}

    string getTitle() { return title; }
    string getAuthor() { return author; }
    bool getIsElectronic() { return isElectronic; }
};
// 图书管理类
class Library {
private:
    vector<Book> books;

public:
    void addBook(Book book) {
        books.push_back(book);
    }

    void removeBook(int index) {
        if (index >= 0 && index < books.size()) {
            books.erase(books.begin() + index);
        } else {
            cout << "索引无效." << endl;
        }
    }

    void displayBooks() {
        cout << "图书馆提供的书籍:" << endl;
        for (int i = 0; i < books.size(); ++i) {
            cout << i + 1 << ". " << books[i].getTitle() << " by " << books[i].getAuthor();
            if (books[i].getIsElectronic()) {
                cout << " (电子书籍)" << endl;
            } else {
                cout << " (纸质书籍)" << endl;
            }
        }
    }
};
// 用户类
class User {
protected:
    string username;
    string password;

public:
    User(string _username, string _password) : username(_username), password(_password) {}

    virtual void login() = 0;
};
// 管理员
class Admin : public User {
private:
    Library& library;

public:
    Admin(string _username, string _password, Library& _library) : User(_username, _password), library(_library) {}

    void login() override {
        string inputUsername, inputPassword;
        cout << "管理员登录" << endl;
        cout << "用户名: ";
        cin >> inputUsername;
        cout << "密码: ";
        cin >> inputPassword;
        if (inputUsername == username && inputPassword == password) {
            cout << "登录成功." << endl;
            manageLibrary();
        } else {
            cout << "用户名或密码不正确" << endl;
        }
    }
// 管理员菜单
    void manageLibrary() {
        int choice;
        do {
            cout << "\n管理员菜单:" << endl;
            cout << "1. 添加书籍" << endl;
            cout << "2. 删除书" << endl;
            cout << "3. 展示书籍" << endl;
            cout << "4. 注销" << endl;
            cout << "输入您的选择: ";
            cin >> choice;
            switch (choice) {
                case 1: {
                    string title, author;
                    bool isElectronic;
                    cout << "输入书名: ";
                    cin.ignore();
                    getline(cin, title);
                    cout << "输入作者: ";
                    getline(cin, author);
                    cout << "是电子的吗? (0 表示否,1 表示是): ";
                    cin >> isElectronic;
                    library.addBook(Book(title, author, isElectronic));
                    cout << "已成功添加." << endl;
                    break;
                }
                case 2: {
                    int index;
                    cout << "输入要删除的书籍索引: ";
                    cin >> index;
                    library.removeBook(index - 1);
                    break;
                }
                case 3:
                    library.displayBooks();
                    break;
                case 4:
                    cout << "正在注销..." << endl;
                    break;
                default:
                    cout << "无效的选择。" << endl;
            }
        } while (choice != 4);
    }
};
// 教师类,继承自用户类
class Teacher : public User {
private:
    Library& library;

public:
    Teacher(string _username, string _password, Library& _library) : User(_username, _password), library(_library) {}

    void login() override {
        string inputUsername, inputPassword;
        cout << "教师登录" << endl;
        cout << "用户名: ";
        cin >> inputUsername;
        cout << "密码: ";
        cin >> inputPassword;
        if (inputUsername == username && inputPassword == password) {
            cout << "登录成功." << endl;
            borrowBooks();
        } else {
            cout << "用户名或密码不正确." << endl;
        }
    }

    void borrowBooks() {
        library.displayBooks();
        // 老师借阅
    }
};
// 学生类,继承自用户类
class Student : public User {
private:
    Library& library;

public:
    Student(string _username, string _password, Library& _library) : User(_username, _password), library(_library) {}

    void login() override {
        string inputUsername, inputPassword;
        cout << "学生登录" << endl;
        cout << "用户名: ";
        cin >> inputUsername;
        cout << "密码: ";
        cin >> inputPassword;
        if (inputUsername == username && inputPassword == password) {
            cout << "登陆成功." << endl;
            borrowBooks();
        } else {
            cout << "用户名或密码不正确." << endl;
        }
    }

    void borrowBooks() {
        library.displayBooks();
        //学生借阅
    }
};
// 主函数
int main() {
    Library library;
    Admin admin("qwt", "666", library);
    Teacher teacher("t", "123", library);
    Student student("s", "123", library);

    // 添加图书库
    library.addBook(Book("Introduction to C++", "John Smith", false));
    library.addBook(Book("Machine Learning Basics", "Alice Johnson", true));
    int userType;
    do {
        cout << "\n用户类型:" << endl;
        cout << "1. 管理员" << endl;
        cout << "2. 教师" << endl;
        cout << "3. 学生" << endl;
        cout << "4. 退出" << endl;
        cout << "请输入您的身份类型: ";
        cin >> userType;
        switch (userType) {
            case 1:
                admin.login();
                break;
            case 2:
                teacher.login();
                break;
            case 3:
                student.login();
                break;
            case 4:
                cout << "退出..." << endl;
                break;
            default:
                cout << "无效的选择." << endl;
        }
    } while (userType != 4);

    return 0;
}
