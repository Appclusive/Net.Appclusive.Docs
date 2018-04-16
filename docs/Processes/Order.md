# Order Process

Client->Api: GET Catalogue  
Api->Client: Catalogue

Client->Api: GET CatalogueItem  
Api->Client: CatalogueItem

Client->Api: POST Cart (Name, Description)  
Api->Client: Cart

Client->Api: POST CartItem  
_note right of Client: Configuration, Name, CartId, CatalogueItemId_  
_note right of Client: !!!Validate Configuration!!!_  
Api->Client: CartItem  

Client->Api: POST Orders/Create (CartId)  
Api->OrderManager: Create(CartId)  
OrderManager->CartDataManager: Get(CartId)  
CartDataManager->OrderManager: Cart  
OrderManager->JobManager: Create  
JobManager->OrderManager: Job  
OrderManager->OrderManager: Create

loop For every CartItem  
    - OrderManager->CatalogueItemManager: Get(cartItem.CatalogueItemId)  
    - CatalogueItemManager->OrderManager: CatalogueItem  
    - OrderManager->BlueprintManager: Get(catalogueItem.BlueprintId)  
    - BlueprintManager->OrderManager: Blueprint  
    - OrderManager->JobManager: Create  
    - JobManager->OrderManager: Job  
    - OrderManager->OrderItemManager: Create  
end

OrderManager->CartManager: Delete  

loop For every OrderItem  
    - OrderManager->BlueprintManager: Get(orderItem.BlueprintId)  
    - BlueprintManager->OrderManager: Blueprint  
    - OrderManager->ModelManager: Get(blueprint.ModelId)  
    - ModelManager->OrderManager: Model  
    - loop For every configuration entry  
          - OrderManager->ModelAttributeManager: Get(configEntry.Id)  
          - ModelAttributeManager->OrderManager: ModelAttribute  
    - end  
    - OrderManager->WorkflowManager: Invoke(activity, inputs)  
end  
OrderManager->Api: Job  
Api->Client: Job

WorkflowManager->Activity: Invoke(inputs)  
Activity->Queue: SendMessage(inputs)


loop  
    - Activity->Queue: Check for message  
    - Queue->Activity: Message  
end

MessageProcessor->Queue: GetMessage  
Queue->MessageProcessor: Message  
MessageProcessor->ModelManager: GetModel(modelName)  
ModelManager->MessageProcessor: Model  
MessageProcessor->ItemManager: Create  
ItemManager->MessageProcessor: Item  
MessageProcessor->AttributeManager: Create
