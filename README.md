# Cocotecho


#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Order {
    int order_num;
    int weight;
    string city;
    int pin_code;
};

struct Container {
    string city;
    int pin_code;
    int weight;
    vector<Order> orders;
};

vector<Order> generateOrders(int numOrders) {
    vector<Order> orders;
    vector<string> cities = {"C1", "C2", "C3", "C4", "C5"};

    for (int orderNum = 1; orderNum <= numOrders; ++orderNum) {
        int weight = rand() % 100 + 1;
        string city = cities[rand() % cities.size()];
        int pinCode = rand() % 1000 + 1000;

        Order order = {orderNum, weight, city, pinCode};
        orders.push_back(order);
    }

    return orders;
}


vector<Container> arrangeIntoContainers(const vector<Order>& orders) {
    vector<Container> containers;
    Container currentContainer = {"", 0, 0, {}};

    for (const auto& order : orders) {
        if (order.city != currentContainer.city || order.pin_code != currentContainer.pin_code ||
            currentContainer.weight + order.weight > 1000) {
            
            containers.push_back(currentContainer);
            currentContainer = {order.city, order.pin_code, 0, {}};
        }

        currentContainer.weight += order.weight;
        currentContainer.orders.push_back(order);
    }

    containers.push_back(currentContainer);  

    return containers;
}


void displayContainers(const vector<Container>& containers) {
    for (int i = 0; i < containers.size(); ++i) {
        const auto& container = containers[i];
        cout << "Container " << i + 1 << " - City: " << container.city << ", Pin Code: " << container.pin_code
             << ", Total Weight: " << container.weight << " kg\n";

        for (const auto& order : container.orders) {
            cout << "  Order " << order.order_num << " - Weight: " << order.weight << " kg\n";
        }
    }
}

int main() {
    srand(time(0));  
  
    vector<Order> orders = generateOrders(1000);

    
    vector<Container> containers = arrangeIntoContainers(orders);

   
    displayContainers(containers);

    return 0;
}






