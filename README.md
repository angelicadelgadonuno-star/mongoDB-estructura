🗄️ Proyecto: Diseño de Bases de Datos NoSQL con MongoDB
Este repositorio contiene el diseño, modelado e implementación de tres bases de datos documentales utilizando MongoDB y MongoDB Compass. El proyecto está dividido en tres niveles de dificultad, demostrando el dominio de patrones de diseño como la referenciación (referencing), el embebido (embedding) y las relaciones Muchos a Muchos (N:M).

🛠️ Tecnologías Utilizadas
Base de Datos: MongoDB Community Server (v9.0.4)

Interfaz Gráfica: MongoDB Compass

Herramienta de Modelado: Compass Data Modeling (Diagramas de relaciones)

Herramientas de Terminal: Homebrew, Atlas CLI, Docker (para despliegue del entorno local).

📚 Nivel 1: Óptica "Bottom_Glass_Optic"
Objetivo: Modelar la gestión de clientes, proveedores, gafas y ventas.

Decisiones de diseño:

Referenciación: Se utilizan referencias (ObjectId) para conectar las gafas con su proveedor y las ventas con el cliente/empleado, evitando la duplicación de datos que pueden cambiar (como el teléfono de un proveedor).

Embebido Histórico: En la colección sales, los datos de las gafas vendidas se embeben dentro de un array. Esto asegura que, si el precio de una gafa cambia en el catálogo en el futuro, la factura del cliente mantenga el precio original del momento de la compra.

Relación Recursiva: La colección Client incluye un campo recommended_by que guarda el ObjectId de otro cliente, resolviendo la recomendación de "boca a boca".

🍔 Nivel 2: Restaurante 

Objetivo: Diseñar el sistema de pedidos de comida a domicilio y gestión de tiendas.

Decisiones de diseño:

Jerarquía de Tiendas: La colección Employee contiene un store_id, estableciendo que un empleado solo puede pertenecer a una tienda, mientras que una tienda puede tener múltiples empleados.

Gestión de Pedidos: La colección orders guarda un array con los productos solicitados (nombre, cantidad y precio). Al igual que en el Nivel 1, se embeben para mantener el histórico de precios.

Lógica Condicional: Los pedidos a domicilio incluyen el delivery_person_id y delivery_date. Si el pedido es para recoger en tienda, estos campos se establecen en null, reflejando la flexibilidad de los documentos en MongoDB.

📺 Nivel 3: Mini-YouTube

Objetivo: Crear un ecosistema complejo de usuarios, vídeos, canales y suscripciones.

Decisiones de diseño:

Relaciones N:M (Muchos a Muchos): La colección playlist utiliza un array de referencias (video_ids) para permitir que un vídeo esté en múltiples playlists y viceversa.

Colecciones de Interacción: Para evitar sobrecargar el documento del vídeo (límite de 16MB de MongoDB), se creó la colección Likes_Dislikes. Esta guarda el user_id, el video_id y el type (like/dislike). Esto asegura que un usuario solo pueda votar una vez por vídeo.

Tabla Pivote en NoSQL: La colección subscriptions actúa como puente entre User y channel, demostrando cómo resolver relaciones de suscripción sin anidar datos innecesarios.

💡 Conclusiones de Modelado (Buenas Prácticas Aplicadas)
Atomicidad vs. Duplicación: Se ha optado por embeber datos (como los productos en un pedido o los vídeos en una playlist) cuando estos representan un "histórico" o "instantánea" del momento, y se ha optado por referenciar (IDs) cuando los datos son entidades independientes que pueden cambiar o crecer indefinidamente.

Flexibilidad de Esquema: Se ha aprovechado la naturaleza schemaless de MongoDB para permitir campos opcionales (como recommended_by o delivery_person_id) que solo se rellenan bajo ciertas condiciones del negocio.

Integridad Referencial Lógica: Aunque MongoDB no impone claves foráneas estrictas, el modelo de datos y los diagramas visuales (Data Modeling) garantizan que todas las referencias apunten a documentos existentes en sus respectivas colecciones.
