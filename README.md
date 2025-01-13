## Regras
- Todos os ids de relacionais vão ser os public_id considerando ser um dado que vai ser exposto
- Todos os ids increment não podem estar nos dtos

## Padrões do banco
Esses padrões tem que ser centralizados em algum local do sistema, sendo carregado por cada um das nossas entidades como herança, para faciitar a manutenção e escrita dos códigos

- Vamos utilizar o "public_id" uuid v4 devido não ser human readble.
- Vamos utilizar o "id" increment para ter um sort caso necessário utilizar por inserção
- created_at timestamp default now
- updated_at timestamp default now sempre que atualizar adicionar um timestamp now
- usable boolean default true vai ser usado para caso um usuário realizar uma deleção, mantendo o dado sem alterar status

## Tables

## corporates
- types enum "carrier" | "riskManager"
- support_wpp boolean
- document string unique
- document_uri string 
- corporate_name string 
- email string unique
- fantasy_name string
- phone string 
- address_id string one to one
- address_complement string
- proof_address string
- status enum "active" | "pending" | "reprove" | "inactive"
- user_ids one to many users

## addresses
- type enum "plusCode" | "publicPlace" | "latLng"
- lat float
- lng flaot
- alias? string
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
- permission_id one to one permissions
- status "active" | "inactive"
- is_master? boolean
- corporate_id? one to one corporates
- avatar_uri string

## alerts
- lat float
- lng flaot
- status enum "peding" | "resolve"
- trip_id one to one
- details? string
- comment? string
- driver_id one to one
- corporate_id one to one corporates
- quantity int default 0

## alerts_settings
- name string
- data JSON (irei fazer um json de example dps)

## drivers
- name string
- one_signal_id string
- address_id one to one addresses 
- address_complement string
- document string unique
- document_uri
- rg? string unique
- rg_uri? string
- date_birth string
- gender string
- email string unique
- phone string
- avatar_uri string
- password string
- token? string
- status enum "active" | "pending" | "reprove" | "inactive"
- rated? int

## rattings
- corporate_id one to one corporates
- driver_id one to one drivers
- details string
- trip_id one to one trips
- rated int

## trips
- route_id one to one routes
- star_point_id one to one startReturnPoints
- start_point_data json
  {
    quantity number
    price float
  }
- return_point_id one to one startReturnPoints
- return_point_data json
  {
    quantity number
    price float
  }
- driver_id one to one drivers
- distancy string
- time_travel string
- status enum "pending" | "enRoute" | "delivery" | "shipping" | "food" | "supply" | "overnight" | "finish"
- services_data JSON
  (estou formulando ainda o JSON)
- vehicle_id one to one vehicles
- implements one to many vehicles
- deliverys_shippings_data JSON
- trip_services_ids one to many tripServices
- trip_deliverys_shippings_ids one to many trip_deliverys_shippings
- checklist_id one to one checklists
- checklist_answer JSON

## start_return_points
- address_id one to one addresses
- corporate_id one to one corporates
- document string
- name string
- phone string
- status enum "inactive" | "active"

## trip_services
- trip_id one to one trips
- price? float
- required boolean
- type enum "food" | "supply" | "overnight"
- service_id one to one services

## trip_deliverys_shippings
- trip_id one to one trips
- deliverys_shippings_id one to one deliverysShippings
- type enum "delivery" | "shipping"
- quantity number
- price float

## services
- corporate_id one to one corporates
- document string
- name string
- types enum "food" | "supply" | "overnight"

## deliverys_shippings
- corporate_id one to one corporates
- types enum "delivery" | "shipping"
- document string
- name string

## routes
- corporate_id one to one corporates
- name string
- star_point_id one to one startReturnPoints
- return_point_id one to one startReturnPoints
- distancy string
- time_travel string
- routes_services_ids one to many routes_services
- routes_deliverys_shippings_ids one to many routes_deliverys_shippings
- additional_way_points JSON

## routes_services
- route_id one to one route
- price float
- type enum "food" | "supply" | "overnight"
- service_id one to one services

## routes_deliverys_shippings
- route_id one to one route
- type enum "delivery" | "shipping"
- quantity number
- price float

## permissions
- users_id one to many users
- corporate_id? one to one corporates
- name string
- data JSON Array<string>

## tokens
- user_id one to one
- token string

## drivers_big_data
- driver_id one to one drivers
- data JSON (varia de acordo com o Big Data n se preocupar)

## driver_devices
- driver_id one to one drivers
- data JSON (n sei o JSON)

## corporates_big_data
- corporate_id one to one corporates
- data JSON (varia de acordo com o Big Data n se preocupar)

## check_lists
- corporate_id one to one corporates
- name
- status enum "inactive" | "active"
- questions JSON
  {
    type enum "text" | ""
  }

## drivers_comporate
- corporate_id one to one corporates
- driver_id one to one drivers
- status enum "pending" | "aprove" | "reprove"

## trip_tracking
- lat float
- lng flaot
- speed float
- signalForce float
- road_speed
- trip_id one to one trips
- driver_id one to one drivers