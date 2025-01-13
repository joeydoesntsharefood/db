## Regras
- Todos os ids de relacionais vão ser os publicId considerando ser um dado que vai ser exposto
- Todos os ids increment não podem estar nos dtos

## Padrões do banco
Esses padrões tem que ser centralizados em algum local do sistema, sendo carregado por cada um das nossas entidades como herança, para faciitar a manutenção e escrita dos códigos

- Vamos utilizar o "publicId" uuidv4 devido não ser human readble.
- Vamos utilizar o "id" increment para ter um sort caso necessário utilizar por inserção
- createdAt timestamp default now
- updatedAt timestamp default now sempre que atualizar adicionar um timestamp now
- usable boolean default true vai ser usado para caso um usuário realizar uma deleção, mantendo o dado sem alterar status

## Tables

## corporates
- type enum "carrier" | "riskManager"
- supportWpp boolean
- document string unique
- documentURI string 
- corporateName string 
- email string unique
- fantasyName string
- phone string 
- addressId string one to one
- proofAddress string
- addressComplement string
- document string
- status enum "active" | "pending" | "reprove" | "inactive"
- userIds one to many users

## addresses
- plusCode
- type enum "plusCode" | "publicPlace" | "latLng"
- lat float
- lng flaot
- data JSON (contem os inseridos do endereco podendo ser 3 tipos)
  
  publicPlace
  {
    street string
    number number | null
    neighborhood string
    city string
    state string
    country string
    zipCode string
  }
  
  plusCode
  {
    value string
  }
  
  latLng
  {
    lat float
    lng float
  }

## users
- name string
- document string unique
- token string
- email string unique
- phone string
- password string
- permissionId one to one permissions
- status "active" | "inactive"
- isMaster? boolean
- corporateId? one to one corporates
- avatarUri string

## alerts
- lat float
- lng flaot
- status enum "peding" | "resolve"
- tripId one to one
- details? string
- comment? string
- driverId one to one
- corporateId one to one corporates

## alertsSettings

## drivers
- name string
- oneSignalId string
- addressId
- addressComplement string
- document string unique
- documentURI
- rg? string unique
- dateBirth string
- gender string
- email string unique
- phone string
- avatarUri string
- password string
- token? string
- status enum "active" | "pending" | "reprove" | "inactive"
- rated? int

## trips
- routeId one to one routes
- starPoint one to one startReturnPoints
- startPointData json
  {
    quantity number
    price float
  }
- returnPoint one to one startReturnPoints
- returnPointData
  {
    quantity number
    price float
  }
- driverId one to one drivers
- distancy string
- timeTravel string
- status enum "pending" | "enRoute" | "delivery" | "shipping" | "food" | "supply" | "overnight" | "finish"
- servicesData JSON
  (estou formulando ainda o JSON)
- vehicleId one to one vehicles
- implements one to many vehicles
- deliverysShippingsData JSON
- tripServicesIds one to many tripServices
- tripDeliverysShippingsIds one to many tripDeliverysShippings

## startReturnPoints
- addressId one to one addresses
- 

## services

## tripServices

## deliverysShippings

## tripDeliverysShippings

## routes

## permissions

## tokens

## driversBigData

## parameters

## driverDevices

## corporatesBigData

## checklists