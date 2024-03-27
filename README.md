# Decentralized-Online-Marketplace-for-Artisans
Créez une place de marché en ligne décentralisée pour les artisans, en utilisant des contrats intelligents pour garantir des paiements équitables et la protection des droits de propriété intellectuelle.
from datetime import datetime
import hashlib
import json

class SmartContract:
    def __init__(self, artist, buyer, item, price):
        self.artist = artist
        self.buyer = buyer
        self.item = item
        self.price = price
        self.contract_created_at = datetime.now()
        self.is_active = True
    
    def execute_contract(self):
        if self.is_active:
            print(f"Contract for {self.item} executed at {self.contract_created_at}.")
            print(f"{self.buyer} paid {self.artist} {self.price}$.")
            self.is_active = False
            return True
        else:
            print("Contract is no longer active.")
            return False

    def validate_rights(self):
        if self.is_active:
            print(f"{self.artist}'s rights for {self.item} are protected under this contract.")
        else:
            print(f"{self.item} has already been purchased by {self.buyer}, rights transferred.")

class Marketplace:
    def __init__(self):
        self.items = {}
        self.contracts = []
    
    def add_item(self, artist, item_name, price):
        item_id = hashlib.sha256(item_name.encode()).hexdigest()
        self.items[item_id] = {"artist": artist, "item_name": item_name, "price": price}
        print(f"Item {item_name} added to marketplace by {artist}.")

    def buy_item(self, buyer, item_id):
        if item_id in self.items:
            item = self.items[item_id]
            contract = SmartContract(item["artist"], buyer, item["item_name"], item["price"])
            self.contracts.append(contract)
            contract.execute_contract()
        else:
            print("Item not found.")

    def list_items(self):
        for item_id, details in self.items.items():
            print(f"ID: {item_id} | Item: {details['item_name']} | Artist: {details['artist']} | Price: {details['price']}$")

# Demo
marketplace = Marketplace()
marketplace.add_item("Alice Artisan", "Handcrafted Vase", 100)
marketplace.add_item("Bob Blacksmith", "Forged Iron Gate", 2000)

marketplace.list_items()

buyer = "Charlie Collector"
item_to_buy = next(iter(marketplace.items))  # Simulating choosing the first item in the list
marketplace.buy_item(buyer, item_to_buy)

contract = marketplace.contracts[0]
contract.validate_rights()
