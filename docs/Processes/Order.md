# Order Process

Client->Api: GET Catalogue\s\s
Api->Client: Catalogue

Client->Api: GET CatalogueItem\s\s
Api->Client: CatalogueItem

Client->Api: POST Cart (Name, Description)\s\s
Api->Client: Cart

Client->Api: POST CartItem\s\s
_note right of Client: Configuration, Name, CartId, CatalogueItemId_\s\s
_note right of Client: !!!Validate Configuration!!!_\s\s
Api->Client: CartItem\s\s

Client->Api: POST Orders/Create (CartId)\s\s
Api->OrderManager: Create(CartId)\s\s
OrderManager->CartDataManager: Get(CartId)\s\s
CartDataManager->OrderManager: Cart\s\s
OrderManager->JobManager: Create\s\s
JobManager->OrderManager: Job\s\s
OrderManager->OrderManager: Create

loop For every CartItem\s\s
    OrderManager->CatalogueItemManager: Get(cartItem.CatalogueItemId)\s\s
    CatalogueItemManager->OrderManager: CatalogueItem\s\s
    OrderManager->BlueprintManager: Get(catalogueItem.BlueprintId)\s\s
    BlueprintManager->OrderManager: Blueprint\s\s
    OrderManager->JobManager: Create\s\s
    JobManager->OrderManager: Job\s\s
    OrderManager->OrderItemManager: Create\s\s
end

OrderManager->CartManager: Delete\s\s

loop For every OrderItem\s\s
    OrderManager->BlueprintManager: Get(orderItem.BlueprintId)\s\s
    BlueprintManager->OrderManager: Blueprint\s\s
    OrderManager->ModelManager: Get(blueprint.ModelId)\s\s
    ModelManager->OrderManager: Model\s\s
    loop For every configuration entry\s\s
        OrderManager->ModelAttributeManager: Get(configEntry.Id)\s\s
        ModelAttributeManager->OrderManager: ModelAttribute\s\s
    end\s\s
    OrderManager->WorkflowManager: Invoke(activity, inputs)\s\s
end\s\s
OrderManager->Api: Job\s\s
Api->Client: Job

WorkflowManager->Activity: Invoke(inputs)\s\s
Activity->Queue: SendMessage(inputs)


loop\s\s
    Activity->Queue: Check for message\s\s
    Queue->Activity: Message\s\s
end

MessageProcessor->Queue: GetMessage\s\s
Queue->MessageProcessor: Message\s\s
MessageProcessor->ModelManager: GetModel(modelName)\s\s
ModelManager->MessageProcessor: Model\s\s
MessageProcessor->ItemManager: Create\s\s
ItemManager->MessageProcessor: Item\s\s
MessageProcessor->AttributeManager: Create
