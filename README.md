# Redes-para-Developers
## 🧩 1. Conceptos Base

| Concepto | Definición simple | Nivel técnico breve | Ejemplo real |
|---|---|---|---|
| **Modelo OSI** | Es un modelo que describe cómo viaja la información entre dispositivos. Divide el proceso en 7 capas ordenadas. | Estándar internacional que organiza los protocolos de red en capas independientes. | Cuando mandas un mensaje por WhatsApp, pasa por todas las capas del modelo OSI. |
| **Subred (Subnet)** | Es una división de una red grande en redes más pequeñas. Ayuda a organizar y controlar el tráfico. | Segmento de red definido por una máscara de subred (ej: `192.168.1.0/24`). | En una empresa, el área de RRHH y el área de IT tienen subredes separadas. |
| **DNS** | Es el servicio que traduce nombres de dominio (como `google.com`) a direcciones IP. | Sistema distribuido de resolución de nombres usando registros A, CNAME, MX, etc. | Escribes `github.com` en el navegador y DNS lo convierte a `140.82.112.4`. |

---

## 🧱 2. Modelo OSI

### ¿Qué es el modelo OSI?
Es un modelo conceptual creado por la ISO que divide en **7 capas** la forma en que los datos viajan de un dispositivo a otro en una red. Cada capa tiene una responsabilidad específica y trabaja con la capa de arriba y de abajo.

### ¿Para qué sirve?
Sirve para entender, diseñar y resolver problemas de comunicación en redes. Cuando algo falla, el modelo OSI te ayuda a identificar **en qué capa está el problema**.

| Capa | Nombre | ¿Qué hace? |
|---|---|---|
| 1 | **Física** | Transmite los bits como señales eléctricas, de luz o radio por el medio físico (cables, wifi). |
| 2 | **Enlace** | Organiza los bits en tramas y controla la comunicación entre dispositivos en la misma red local (MAC address). |
| 3 | **Red** | Se encarga del direccionamiento y enrutamiento de los datos entre redes distintas (IP address). |
| 4 | **Transporte** | Garantiza que los datos lleguen completos y en orden. Usa protocolos como TCP y UDP. |
| 5 | **Sesión** | Establece, mantiene y cierra la comunicación entre dos aplicaciones. |
| 6 | **Presentación** | Traduce, comprime y cifra los datos para que la aplicación los entienda (ej: SSL/TLS, JSON). |
| 7 | **Aplicación** | Es lo que el usuario usa directamente: navegadores, apps, correo. Protocolos como HTTP, FTP, SMTP. |

> 💡 **Clave:** No lo memorices, entiende el flujo. Los datos bajan por las capas al enviar y suben al recibir.

---

## 3. DNS — Lo que usas todos los días

### ¿Qué es DNS?
DNS (*Domain Name System*) es como la agenda de contactos de internet. Convierte nombres legibles para humanos (`google.com`) en direcciones IP que las máquinas entienden (`142.250.185.14`).

### ¿Por qué no usamos IP directamente?
Porque las IPs son difíciles de recordar y pueden cambiar. Es mucho más fácil recordar `github.com` que `140.82.112.4`. Además, un dominio puede apuntar a distintas IPs según la región o carga del servidor.

### ¿Qué pasa cuando escribes `google.com` en el navegador?


**Flujo resumido:** `Usuario → DNS → IP → Servidor → Respuesta`

---

## 4. Subnets — Nivel básico pero clave

### ¿Qué es una subred?
Una subred es una porción de una red más grande. Divide una red en segmentos más pequeños y manejables.

### ¿Para qué sirve?
- Organizar dispositivos por áreas o funciones
- Mejorar la seguridad (aislar partes de la red)
- Optimizar el tráfico y el rendimiento

### ¿Por qué dividir redes?
Porque no todos los dispositivos necesitan comunicarse entre sí. Separar reduce riesgos de seguridad y hace más eficiente la administración.

### 🖥️ Ejemplo developer: Red local vs Cloud

| Escenario | Ejemplo |
|---|---|
| **Red local** | Tu computador en `192.168.1.5` está en la misma subred que tu impresora en `192.168.1.10`. |
| **Cloud (AWS/GCP)** | Tu app en producción vive en una subred privada (`10.0.1.0/24`) y solo el load balancer tiene IP pública. |

---

## 5. Conexión directa con desarrollo 🔌

### ¿Qué pasa cuando tu app...

**No encuentra el servidor:**
> Puede ser un problema de DNS (el dominio no resuelve) o de red (la IP no es alcanzable). Revisa si el servidor está activo y si el nombre de host está bien escrito.

**No resuelve el dominio:**
> El DNS está fallando. Puede ser que el registro DNS esté mal configurado, que el dominio haya expirado o que el servidor DNS esté caído.

**No puede conectarse:**
> La IP existe y resuelve, pero el puerto está bloqueado, el firewall rechaza la conexión o el servicio no está corriendo.

| Problema | Relación |
|---|---|
| No resuelve dominio | **DNS** mal configurado o caído |
| IP incorrecta | **IP** apuntando al servidor equivocado |
| Sin acceso a la app | **Red** bloqueada por firewall o subred |

---

## 6. Problemas reales — Debugging

| Error | ¿Qué significa? | ¿Qué revisarías primero? |
|---|---|---|
| **DNS not found** | El dominio no pudo resolverse a una IP. | Verifica el registro DNS, que el dominio esté activo y prueba con `nslookup dominio.com`. |
| **Host unreachable** | La IP existe pero no hay ruta de red para llegar a ella. | Revisa la configuración de red, firewall, y si el servidor está encendido. |
| **Network error** | Error genérico de conexión, puede ser DNS, IP, puerto o firewall. | Prueba paso a paso: DNS → ping a IP → telnet al puerto → revisar logs del servidor. |

---

## 7. Caso Práctico Real

> **Escenario:** *"Tu aplicación está deployada, pero nadie puede acceder."*

### ¿Es problema de DNS?
Sí, si el dominio no resuelve. Verifica con `nslookup tuapp.com` o `dig tuapp.com`. Revisa que el registro A apunte a la IP correcta.

### ¿Es problema de red?
Sí, si el dominio resuelve pero no hay conexión. Haz `ping` a la IP. Si no responde, el servidor puede estar caído o el firewall bloqueando el tráfico.

### ¿Es problema de configuración?
Sí, si la conexión llega pero la app no responde. Revisa que el servidor web esté corriendo (nginx, apache, tu app), que el puerto correcto esté abierto (80/443) y que las variables de entorno estén bien configuradas.

> 🔍 **Orden de debugging recomendado:** DNS → Red → Puerto → App → Logs

---

## 🧪 8. Analogía obligatoria

| Concepto | Analogía |
|---|---|
| **OSI** | Es como enviar una carta: la escribes, la metes en un sobre, la llevas al correo, pasa por distintas estaciones hasta llegar al destino, donde el proceso se hace al revés para abrirla y leerla. |
| **DNS** | Es como una agenda de contactos: no marcas el número de teléfono de memoria, buscas el nombre de tu amigo y la agenda te da el número. |
| **Subnet** | Es como dividir una ciudad en barrios: todos están en la misma ciudad, pero cada barrio tiene su zona, sus reglas y sus accesos. |

---

## 🧠 BONUS — Nivel Bootcamp Pro

### ¿Qué es un VPC en cloud?
Un **VPC** (*Virtual Private Cloud*) es una red privada virtual dentro de un proveedor cloud como AWS o GCP. Te permite aislar tus recursos (servidores, bases de datos) del resto del internet, definir tus propias subredes y controlar quién puede acceder a qué.

### ¿Qué es una IP privada vs pública?

| Tipo | Descripción | Ejemplo |
|---|---|---|
| **IP Privada** | Solo visible dentro de una red local o VPC. No accesible desde internet. | `192.168.1.5`, `10.0.0.4` |
| **IP Pública** | Visible desde internet. Es la dirección con la que tu servidor se comunica con el mundo. | `186.64.120.1` |

> En producción, tu base de datos tiene IP privada (segura) y tu servidor web tiene IP pública (accesible).

### ¿Qué es un Load Balancer?
Un **load balancer** (balanceador de carga) distribuye el tráfico entrante entre múltiples servidores para que ninguno se sobrecargue. Si tienes 3 servidores corriendo tu app, el load balancer decide cuál atiende cada petición según disponibilidad y carga.

> **Analogía:** Es como la cajera de un supermercado que te dice "pase a la caja 3, que está libre" en lugar de que todos hagan fila en la misma caja.

