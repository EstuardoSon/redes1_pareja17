### **Universidad San Carlos de Guatemala**
### **Facultad de Ingeniería**
### **Escuela de Ciencias y Sistemas**
### **Redes de Computadoras 1 N**
### **Catedrático: Ing. Pedro Pablo Hernández Ramírez**
### **Auxiliar: Edwin López**

## **Manual Técnico - Práctica 1**

- **Estuardo Gabriel Son Mux – 202003894**
- **Angel Eduardo Marroquín Canizales – 202003959**
---
## **Tabla de direcciones**
| Nombre del dispositivo | IP   | Máscara de subred | Área | Nivel |
|------------------------|------|-------------------|------|-------|
|VPC6|192.168.17.1| 255.255.255.0| Almacenamiento | 1 |
|VPC7|192.168.17.2| 255.255.255.0| Almacenamiento | 1 |
|VPC4|192.168.17.3| 255.255.255.0| Recepción | 1 |
|VPC5|192.168.17.4| 255.255.255.0| Recepción | 1 |
|VPC8|192.168.17.5| 255.255.255.0| Atención al cliente | 2 |
|VPC9|192.168.17.6| 255.255.255.0| Atención al cliente | 2 |
|VPC10|192.168.17.7| 255.255.255.0| Atención al cliente | 2 |
|VPC11|192.168.17.8| 255.255.255.0| Atención al cliente | 2 |
|VPC12|192.168.17.9| 255.255.255.0| Oficina administrativa | 2 |
|VPC13|192.168.17.10| 255.255.255.0| Oficina administrativa | 2 |
|VPC14|192.168.17.11| 255.255.255.0| Oficina administrativa | 2 |
|VPC15|192.168.17.12| 255.255.255.0| Gerencia | 3 |
|VPC16|192.168.17.13| 255.255.255.0| Gerencia | 3 |
|VPC17|192.168.17.14| 255.255.255.0| Operaciones | 3 |
|VPC18|192.168.17.15| 255.255.255.0| Operaciones | 3 |
|VPC19|192.168.17.16| 255.255.255.0| Operaciones | 3 |

## **Hardware a usar**

| Dispositivo | Fabricante | Modelo | Cantidad | Precio Unidad | Información Adicional|
|-------------|------------|--------|----------|---------------|------------|
| Switch | INTELLINET | 561068 | 3 | Q703.00 | 16 Puertos autosensitivos 10/100/1000 Mbps |
| Cable UTP Cat 6| Nexxt Solutions| AB356NXT01| 150 m | Q5.49 x m | UTP Cat 6 calibre 23 awg |
| Conector RJ45 | Nexxt Solutions | AW102NXT04 | 38 | Q4.00 | Conector RJ45 categoría 6 |

**Computadoras:**
Se necesitaran de 16 computadoras capaces de conectarse a la red a implementar. El sistema operativo, así como las especificaciones de cada computadora quedan a elección de la empresa.

## **Configuración de VPCs**

### **Nivel 1**
Configurarción del dispositivo VPC6  
```cmd
ip 192.168.17.4 24
```
### **Nivel 2**
Configurarción del dispositivo VPC8  
```cmd
ip 192.168.17.5 24
```
### **Nivel 3**
Configurarción del dispositivo VPC15  
```cmd
ip 192.168.17.12 24
```