## Post Explotación
 <a name="tratamientotty"></a>
 ### Tratamiento TTY
 Una vez obtenido acceso a la máquina, lo primero es conseguir una shell interactiva y completamente funcional, para ello:
 ```
 script /dev/null -c bash
 Ctrl+Z
 stty raw -echo; fg
export TERM=xterm
 export SHELL=bash
 
 ```

<a href="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes">Menú Principal</a>
