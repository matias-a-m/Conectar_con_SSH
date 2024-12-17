# **Guía completa: Conectar un repositorio local con GitHub utilizando SSH**

---

Esta guía detalla cómo establecer una conexión segura y sin errores entre tu repositorio local y GitHub mediante SSH, con pasos claros y verificados.

---

## **1. Verificar claves SSH existentes**

Comprueba si ya tienes claves SSH configuradas.

```bash
ls -al ~/.ssh

```

Si no existe el directorio `~/.ssh`, significa que no tienes claves configuradas.

---

## **2. Generar una nueva clave SSH**

Crea una clave SSH `ed25519` para mayor seguridad.

```bash
ssh-keygen -t ed25519 -C "tu_correo@example.com"

```

- Al generarla, te pedirá un nombre de archivo para guardar la clave:
    - Usa un nombre descriptivo, por ejemplo: `github_key`.
- Si prefieres mayor seguridad, añade una frase de contraseña (opcional).

---

## **3. Configurar permisos de la clave privada**

Asegúrate de que tu clave privada sea accesible solo por ti:

```bash
chmod 600 ~/.ssh/github_key
chmod 700 ~/.ssh

```

---

## **4. Configurar el archivo SSH `config`**

Simplifica el uso de la clave configurando el archivo `~/.ssh/config`.

```bash
nano ~/.ssh/config

```

Añade la siguiente configuración para especificar tu clave:

```bash
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_key

```

Guarda y cierra el archivo (`Ctrl+O`, luego `Ctrl+X` en `nano`).

> Nota: Este archivo permite gestionar múltiples claves SSH si trabajas con varios servidores. Más información en la documentación oficial.
> 

---

## **5. Añadir la clave SSH al agente SSH**

Para habilitar la clave SSH en tu sesión activa:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/github_key

```

---

## **6. Agregar la clave pública a GitHub**

Copia el contenido de tu clave pública y agrégala a tu cuenta de GitHub:

```bash
cat ~/.ssh/github_key.pub

```

1. Ve a [GitHub → Configuración → SSH and GPG keys](https://github.com/settings/keys).
2. Haz clic en **New SSH key**.
3. Asigna un título (por ejemplo, `github_key`) y pega la clave pública.
4. Guarda los cambios.

---

## **7. Probar la conexión SSH**

Verifica que la conexión con GitHub sea exitosa:

```bash
ssh -T git@github.com

```

### **Si la conexión falla (Permission denied)**

1. Verifica que el agente tenga la clave correcta:
Si no aparece tu clave, añádela:
    
    ```bash
    ssh-add -l
    
    ```
    
    ```bash
    ssh-add ~/.ssh/github_key
    
    ```
    
2. Revisa la configuración del archivo `config` para posibles errores.
3. Consulta los logs detallados con:
Esto ayudará a identificar problemas específicos.
    
    ```bash
    ssh -vT git@github.com
    
    ```
    

---

## **8. Configurar el repositorio remoto**

Asegúrate de usar la URL SSH para conectar el repositorio remoto.

1. Configura el repositorio local con la URL SSH:
    
    ```bash
    git remote add origin git@github.com:tu_usuario/tu_repositorio.git
    
    ```
    
2. Si ya tienes un origen configurado:
    
    ```bash
    git remote set-url origin git@github.com:tu_usuario/tu_repositorio.git
    
    ```
    

---

## **9. Probar la conexión al repositorio**

Envía un cambio de prueba para verificar que todo funcione correctamente:

    ```bash
    git push -u origin main
 
    ```
