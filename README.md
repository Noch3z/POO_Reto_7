# POO_Reto_7

This code implemented an iterator for the Orders the Restaurant got. It's a new class where every one of the possible attributes were taken into account. Here is the result:

```
class OrderIterator:
    def __init__(self, order):
        self._order = order
        self._index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self._index < len(self._order):
            item = self._order[self._index]
            self._index += 1
            return {
                'name': item.get_name(),
                'price': item.get_price(),
                'temperature': item.get_temperature() if hasattr(item, 'get_temperature') else None,
                'vegetarian_friendly': item.is_vegetarian_friendly() if hasattr(item, 'is_vegetarian_friendly') else None,
                'diabetic_friendly': item.is_diabetic_friendly() if hasattr(item, 'is_diabetic_friendly') else None,
                'gluten_free': item.is_gluten_free() if hasattr(item, 'is_gluten_free') else None,
                'size': item.get_size() if hasattr(item, 'get_size') else None,
                'is_vegetarian': item.get_is_vegetarian() if hasattr(item, 'get_is_vegetarian') else None,
                'cuisine_type': item.get_cuisine_type() if hasattr(item, 'get_cuisine_type') else None,
                'is_sweet': item.get_is_sweet() if hasattr(item, 'get_is_sweet') else None
            }
        else:
            raise StopIteration

```

This returns "None" for attributes that don't exist. Wanna try it? Run this on your terminal:

```
class MenuItem:
    def __init__(self, name:str, price:float, temperature:str, vegetarian_friendly:bool, diabetic_friendly:bool, gluten_free:bool):
        self.name = name
        self.price = price
        self.temperature = temperature
        self.vegetarian_friendly = vegetarian_friendly
        self.diabetic_friendly = diabetic_friendly
        self.gluten_free = gluten_free

    def get_name(self):
        return self.name

    def set_name(self, name):
        self.name = name

    def get_price(self):
        return self.price

    def set_price(self, price):
        self.price = price

    def get_temperature(self):
        return self.temperature

    def set_temperature(self, temperature):
        self.temperature = temperature

    def is_vegetarian_friendly(self):
        return self.vegetarian_friendly

    def set_vegetarian_friendly(self, vegetarian_friendly):
        self.vegetarian_friendly = vegetarian_friendly

    def is_diabetic_friendly(self):
        return self.diabetic_friendly

    def set_diabetic_friendly(self, diabetic_friendly):
        self.diabetic_friendly = diabetic_friendly

    def is_gluten_free(self):
        return self.gluten_free

    def set_gluten_free(self, gluten_free):
        self.gluten_free = gluten_free

class Beverage(MenuItem):
    def __init__(self, name, price, size:str):
        super().__init__(name, price, None, None, None, None)
        self.size = size

    def get_size(self):
        return self.size

    def set_size(self, size):
        self.size = size

class Appetizer(MenuItem):
    def __init__(self, name, price, is_vegetarian:bool):
        super().__init__(name, price, None, None, None, None)
        self.is_vegetarian = is_vegetarian

    def get_is_vegetarian(self):
        return self.is_vegetarian

    def set_is_vegetarian(self, is_vegetarian):
        self.is_vegetarian = is_vegetarian

class Entree(MenuItem):
    def __init__(self, name, price, cuisine_type:str):
        super().__init__(name, price, None, None, None, None)
        self.cuisine_type = cuisine_type

    def get_cuisine_type(self):
        return self.cuisine_type

    def set_cuisine_type(self, cuisine_type):
        self.cuisine_type = cuisine_type

class Dessert(MenuItem):
    def __init__(self, name, price, is_sweet:bool):
        super().__init__(name, price, None, None, None, None)
        self.is_sweet = is_sweet

    def get_is_sweet(self):
        return self.is_sweet

    def set_is_sweet(self, is_sweet):
        self.is_sweet = is_sweet

    def get_name(self):
        return super().get_name()

    def get_price(self):
        return super().get_price()

class Order:
    def __init__(self, order: list = []):
        self.order = order

    def add_to_order(self, new_item: MenuItem, quantity: int):
        for i in range(quantity):
            self.order.append(new_item)
        return self.order

    def calculate_total_price(self):
        total = 0
        has_entree = False
        has_beverage = False
        has_appetizer = False
        has_dessert = False
        small_beverage = False
        only_dessert_and_small_beverage = True

        for item in self.order:
            if isinstance(item, Entree):
                has_entree = True
            if isinstance(item, Beverage):
                has_beverage = True
                if item.get_size().lower() == "small":
                    small_beverage = True
            if isinstance(item, Appetizer):
                has_appetizer = True
            if isinstance(item, Dessert):
                has_dessert = True

        for item in self.order:
            price = item.get_price()
            if has_entree and isinstance(item, Beverage):
                price *= 0.9
            total += price

        if has_entree and has_beverage and has_appetizer and has_dessert:
            total *= 0.6
        elif has_dessert and small_beverage and len(self.order) == 2:
            total *= 0.7

        return total

    def print_order(self):
        for i in range(len(self.order)):
            print(self.order[i].get_name())

class Payment:
    def __init__(self):
        pass

    def pay(self, amount:int):
        pass

class Card(Payment):
    def __init__(self, card_number:str, cvv:int, expiration_date:str):
        self.card_number = card_number
        self.cvv = cvv
        self.expiration_date = expiration_date

    def pay(self, amount:int):
        print(f"Payment of {amount} was processed with card number {self.card_number}")
        return True

class Cash(Payment):
    def __init__(self, amount:int):
        self.amount = amount

    def pay(self, amount:int):
        if self.amount >= amount:
            print(f"Payment of {amount} was processed with cash")
            return True
        else:
            print(f"Insufficient funds")
            return False

class OrderIterator:
    def __init__(self, order):
        self._order = order
        self._index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self._index < len(self._order):
            item = self._order[self._index]
            self._index += 1
            return {
                'name': item.get_name(),
                'price': item.get_price(),
                'temperature': item.get_temperature() if hasattr(item, 'get_temperature') else None,
                'vegetarian_friendly': item.is_vegetarian_friendly() if hasattr(item, 'is_vegetarian_friendly') else None,
                'diabetic_friendly': item.is_diabetic_friendly() if hasattr(item, 'is_diabetic_friendly') else None,
                'gluten_free': item.is_gluten_free() if hasattr(item, 'is_gluten_free') else None,
                'size': item.get_size() if hasattr(item, 'get_size') else None,
                'is_vegetarian': item.get_is_vegetarian() if hasattr(item, 'get_is_vegetarian') else None,
                'cuisine_type': item.get_cuisine_type() if hasattr(item, 'get_cuisine_type') else None,
                'is_sweet': item.get_is_sweet() if hasattr(item, 'get_is_sweet') else None
            }
        else:
            raise StopIteration

water = Beverage(name="Water", price=1500, size="small")
bandeja_paisa = Entree(name="Bandeja paisa", price=13000, cuisine_type="colombian")
cake = Dessert(name="just cake", price=5000, is_sweet=True)
new_order = Order([water])
new_order.add_to_order(cake, 2)

new_order.print_order()
new_order.add_to_order(bandeja_paisa, 3)
new_order.print_order()
print(new_order.calculate_total_price())

order_iterator = OrderIterator(new_order.order)
for item in order_iterator:
    print(item)
```
