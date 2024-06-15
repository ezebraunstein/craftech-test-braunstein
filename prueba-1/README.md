Al realizar el diseño y arquitectura de la implementación en la nube de esta aplicación web, se decidió hacer uso de las herramientas que nos provee Amazon Web Services. 

Para mejorar la experiencia del usuario, se utilizará un Content Delivery Network (CDN) denominado CloudFront. De esta forma se servirá el contenido estático desde ubicaciones cercanas al usuario final.  CloudFront se integrará con un bucket de Amazon Simple Storage Service (S3) donde se almacenarán todos los archivos estáticos como HTML, CSS y JavaScript. Esto garantizará una reducción en la latencia y mejorará el rendimiento de carga de la página.

Para garantizar la alta disponibilidad, se configurarán diferentes zonas dentro de una única región de AWS. Al ser zonas independientes, se reduce la posibilidad de un fallo simultáneo, ya que operarán como centros de datos independientes.

Para manejar eficientemente las cargas variables, se implementará el Elastic Load Balancer (ELB). ELB gestionará y distribuirá de forma automática el tráfico entre las distintas instancias de EC2, en las diferentes zonas de disponibilidad. Estas instancias de EC2 contarán con Auto Scaling, que permitirá ajustar dinámicamente los recursos disponibles según la carga de trabajo.

En lo que respecta a las bases de datos, el backend interactuará tanto con una base de datos relacional como con una no relacional. Se empleará Amazon Relational Database Service (RDS) para gestionar la base de datos relacional. RDS ofrece compatibilidad con múltiples motores de bases de datos. Además, utilizaremos Amazon DynamoDB para la base de datos no relacional. DynamoDB se caracteriza por su baja latencia y su escalabilidad.

Los microservicios externos se integrarán mediante API Gateway, el cual acturá como un punto de entrada unificado para las APIs, simplificando la administración, monitoreo y seguridad. Además, utilizaremos funciones Lambda entre el Gateway y las APIs. De esta forma se creará una capa de abstracción, resultando en un menor acoplamiento.

Finalmente, se utilizará CloudWatch como herramienta de monitoreo, permitiendo el acceso a métricas de rendimiento y logs de ejecución. Además, Amazon IAM facilitará la gestión de accesos y políticas, filtrando los usuarios y servicios que pueden acceder a nuestros recursos de AWS.
