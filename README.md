[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/_v8RbUGg)
#include <iostream>
#include <string>
#include <cstdlib>
#include <ctime>

using namespace std;

// 定義角色類別
class Character {
public:
    string name;
    int health;
    int attack;
    int defense;

    Character(string n, int h, int a, int d) : name(n), health(h), attack(a), defense(d) {}

    void takeDamage(int damage) {
        int actualDamage = damage - defense;
        if (actualDamage < 0) actualDamage = 0;
        health -= actualDamage;
        if (health < 0) health = 0;
    }
};

// 定義怪物類別
class Monster {
public:
    string name;
    int health;
    int attack;

    Monster(string n, int h, int a) : name(n), health(h), attack(a) {}

    void takeDamage(int damage) {
        health -= damage;
        if (health < 0) health = 0;
    }
};

// 函數: 選擇角色類型
Character selectCharacter() {
    cout << "\n選擇角色類型: " << endl;
    cout << "1. Defender (高防禦, 中攻擊, 中生命值)" << endl;
    cout << "2. Admiral (高攻擊, 中防禦, 中生命值)" << endl;
    int choice;
    cin >> choice;

    if (choice == 1) {
        return Character("Defender", 120, 40, 40);
    } else if (choice == 2) {
        return Character("Admiral", 80, 60, 20);
    } else {
        cout << "無效選擇，預設為 Defender。" << endl;
        return Character("Defender", 160, 50, 30);
    }
}

// 主函數
int main() {
    srand(time(0)); // 初始化隨機數種子

    // 創建角色和怪物
    Character player = selectCharacter();
    Monster dragon("Dragon", 200, 50);

    cout << "\n戰鬥開始!" << endl;
    cout << player.name << " VS " << dragon.name << "\n" << endl;

    // 回合制戰鬥
    int turn = 1;
    while (turn <= 6 && player.health > 0 && dragon.health > 0) {
        cout << "第 " << turn << " 回合:" << endl;
        cout << player.name << " 的生命值: " << player.health << endl;
        cout << dragon.name << " 的生命值: " << dragon.health << endl;

        // 玩家行動
        cout << "選擇行動: " << endl;
        cout << "1. 攻擊" << endl;
        cout << "2. 防禦" << endl;
        cout << "3. 回復" << endl;
        int action;
        cin >> action;

        if (action == 1) {
            // 攻擊
            int damage = player.attack;
            dragon.takeDamage(damage);
            cout << "你攻擊了 " << dragon.name << "，造成 " << damage << " 傷害。" << endl;
        } else if (action == 2) {
            // 防禦
            cout << "你選擇防禦，減少受到的傷害。" << endl;
            player.defense += 20; // 提高防禦
        } else if (action == 3) {
            // 回復
            int heal = 60;
            player.health += heal;
            if (player.health > 300) player.health = 300; // 最大生命值限制
            cout << "你回復了 " << heal << " 點生命值。" << endl;
        } else {
            cout << "無效行動，跳過此回合。" << endl;
        }

        // 怪物行動
        if (dragon.health > 0) {
            int monsterDamage = dragon.attack;
            cout << dragon.name << " 攻擊了你，造成 " << monsterDamage << " 傷害。" << endl;
            player.takeDamage(monsterDamage);
        }

        // 恢復玩家的原始防禦
        if (action == 2) {
            player.defense -= 10;
        }

        // 檢查勝負
        if (dragon.health <= 0) {
            cout << "\n你擊敗了 " << dragon.name << "！恭喜你獲得勝利！" << endl;
            break;
        }
        if (player.health <= 0) {
            cout << "\n你被 " << dragon.name << " 擊敗了。遊戲結束。" << endl;
            break;
        }

        turn++;
    }

    // 判定6回合後的勝負
    if (turn > 6 && dragon.health > 0 && player.health > 0) {
        cout << "\n戰鬥結束，但你未能在6回合內擊敗 " << dragon.name << "。你失敗了。" << endl;
    }

    return 0;
}
