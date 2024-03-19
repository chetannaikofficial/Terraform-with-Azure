# Terraform-with-Azure
This repositry conains the terraform code which are tested and implemented

# Create Virtual Network 
resource "azurerm_virtual_network" "vnet01" {
  name                = myvnet01
  address_space       = ["192.168.0.1/16"]
  resource_group_name = azurerm_resource_group.rg001.name
  location            = azurerm_resource_group.rg001.location
}

# Create subnet and link to the Vnet created
resource "azurerm_subnet" "subnet01" {
  name                 = "mysubnet01"
  address_prefixes     = ["192.168.0.1/24"]
  virtual_network_name = azurerm_virtual_network.vnet01.name
  resource_group_name  = azurerm_resource_group.rg001.name
}


# Create Network Interface card and associate with the subnet created above
resource "azurerm_network_interface" "mynic01" {
  name                = mynic01
  resource_group_name = azurerm_resource_group.rg001.name
  location            = azurerm_resource_group.rg001.location
  ip_configuration {
    name                          = "myprivateIP"
    subnet_id                     = azurerm_subnet.subnet01.id
    private_ip_address_allocation = "Dynamic"
  }
}
