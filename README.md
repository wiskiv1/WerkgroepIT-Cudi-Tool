# De cudi Sale tool werkende krijgen op de werkgroependatabase

Dit document is een handleidin/uitleg van alle problemen die ik heb gevonden tijdens het werken aan de cudi sales tool van de litus en wat ik heb gedaan om deze te verbeteren.

Het doel van dit document is om duidelijk te communiceren wat ik heb gedaan om de sales tool te verbeteren en werkende te krijgen op de werkgroep IT versie van de litus  
Somige van deze wijzigingen dienen dus niet naar de officiele litus gepushed te worden, enkel naar de werkgroep leden, ik zal mijn best doen dit altijd zo duidelijk mogelijk te vermelden  

Je vindt hier ook alle relevante bestanden en code :))

grtjs  
\- Witse

## overzicht  

[Sale session Error](#sales-session-error)  
[Sale Tool Websocket](#sale-tool-websocket)  


## Sales session error
**ENKEL VOOR DE WERKGROEP**  

Bij het aanmaken van een nieuwe Sale session (Cudi > sale sessions > new), krijg je deze error  

![Error](Error%20add%20sale%20session.png)  

> **Oplossing**  
> Een dummy sale session in de database steken zodat de getCloseRegister() functie iets kan returnen  
>
> Database met de implementatie vindt je hier: [werkgroepDatabase_sale_session_fix](/werkgroepDatabase_Sale_Session_fix)  

aangepaste tabellen:  

cudi_sale_sessions  
|id | open_register | close_register | manager | open_date | close_date | comment |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |   
|1 | 1 | 2 | 25321 | 2024-10-22 16:02:44 | 2024-10-22 18:02:50 | "NIET VERWIJDEREN" |

general_bank_cash_registers  

|id|
|--|  
|1|
|2|


general_bank_money_units_amounts  

|id| cash_register_id | unit_id | amount |
|:-: | :-:| :-:| :-: | 
|1| 1 | 1 | 0 |
|2| 1 | 2 | 0 |
|3| 1 | 3 | 0 |
|4| 1 | 4 | 0 |
|5| 1 | 5 | 0 |
|6| 1 | 6 | 0 |
|7| 1 | 7 | 0 |
|8| 1 | 8 | 0 |
|9| 1 | 9 | 0 |
|10| 1 | 10 | 0 |
|11| 1 | 11 | 0 |
|12| 1 | 12 | 0 |
|13| 1 | 13 | 0 |
|14| 1 | 14 | 0 |
|15| 1 | 15 | 0 |
|16| 2 | 1 | 0 |
|17| 2 | 2 | 0 |
|18| 2 | 3 | 0 |
|19| 2 | 4 | 0 |
|20| 2 | 5 | 0 |
|21| 2 | 6 | 0 |
|22| 2 | 7 | 0 |
|23| 2 | 8 | 0 |
|24| 2 | 9 | 0 |
|25| 2 | 10 | 0 |
|26| 2 | 11 | 0 |
|27| 2 | 12 | 0 |
|28| 2 | 13 | 0 |
|29| 2 | 14 | 0 |
|30| 2 | 15 | 0 |

Elke Cash register heeft 15 entries in deze tabel voor de 15 unit types

## Sale tool websocket  
**ENKEL VOOR DE WERKGROEP**

Bij het openen van de sale tool probeert de Sale tool te verbinden met Liv (Hetzner) en niet je lokale websocket

![Sale_Tool_Error](/Sale_tool_Websocket_error.png)  

> **Oplossing**  
> IDK Cous, het adress veranderen? moet nog proberen  
> Eerst phpStorm werkende krijgen op men pc

Probeersels:

in module > CudiBundle > Component > Controller > SaleController.php vind je de getSocketurl() functie => deze functie geeft het adress van de websocket  

```
protected function getSocketUrl()
    {
        return $this->getEntityManager()
            ->getRepository('CommonBundle\Entity\General\Config')
            ->getConfigValue('cudi.queue_socket_public');
    }
```

miss is het mogelijk om de link naar de websocket van de lokale_litus te hardcoderen? dit zou wel geen elegante oplossing zijn, eerder quick en dirty om het snel te laten werken. ¯\\_(ツ)_/¯  
het returnen van ```liv.localhost/websocket/cudi/``` werkt al sowieso niet.

Er is de module > CudiBundle > Command > Socket > Sale.php file, maar weet niet of dit echt relevant is?

Aan de andere kant zijn alle API keys , auth codes en tokens weg, kan het zijn dat we daarom niet kunnen verbinden?