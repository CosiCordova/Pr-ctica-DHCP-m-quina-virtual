# Práctica: DHCP Máquina Virtual

1. **Instalar el paquete isc-dhcp-server:**
    ```bash
    sudo apt-get update
    sudo apt-get install isc-dhcp-server
    ```

2. **Editar `/etc/dhcp/dhcpd.conf` para una configuración básica:**
    Abrí el archivo de configuración en un editor de texto, por ejemplo, con nano:
    ```bash
    sudo nano /etc/dhcp/dhcpd.conf
    ```
    Agregué una configuración básica, por ejemplo:
    ```plaintext
    subnet 172.16.0.0 netmask 255.255.0.0 {
      range 172.16.1.10 172.16.1.50;
      option routers 172.16.1.1;
      option domain-name-servers 8.8.8.8, 8.8.4.4;
    }
    ```
    Guardé y cerré el editor.

3. **Editar `/etc/default/isc-dhcp-server` para configurar la interfaz de red:**
    ```bash
    sudo nano /etc/default/isc-dhcp-server
    ```
    Aseguré de que la interfaz esté configurada para la red interna de VirtualBox, por ejemplo:
    ```plaintext
    INTERFACESv4="enp0s8"
    ```
    Guardé y cerré el editor.

4. **Arrancar el servicio y comprobar el estado:**
    ```bash
    sudo systemctl start isc-dhcp-server
    sudo systemctl status isc-dhcp-server
    ```

5. **Probar con un cliente:**
    Aseguré de tener otra máquina virtual configurada para usar la misma red interna en VirtualBox. El cliente obtuvo una dirección IP automáticamente dentro del rango especificado.

6. **Declarar una asignación por MAC fija:**
    Volví a editar `/etc/dhcp/dhcpd.conf` y añadí una entrada como la siguiente:
    ```plaintext
    host client1 {
      hardware ethernet 08:00:27:XX:XX:XX;  # Sustituí con la dirección MAC del cliente
      fixed-address 172.16.1.5;
    }
    ```
    Guardé y cerré el editor.

7. **Probar con otro cliente:**
    Aseguré de que el segundo cliente tuviera una dirección MAC correspondiente a la entrada que añadí en el paso anterior.

8. **Comprobar con Wireshark:**
    Instalé Wireshark si aún no lo tenía:
    ```bash
    sudo apt-get install wireshark
    ```
    Abrí Wireshark y seleccioné la interfaz de red que está en la red interna de VirtualBox. Pude ver los mensajes del protocolo DHCP.

9. **Reiniciar el servicio si fue necesario:**
    ```bash
    sudo systemctl restart isc-dhcp-server
    ```
