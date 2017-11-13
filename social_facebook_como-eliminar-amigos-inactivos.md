# Facebook - Coo eliminar amigos inactivos

Es interesante ir borrando las cuentas de amigos que ya se dieron de baja, sobre todo si hemos alcanzado el tope de 5000 amigos que nos permite FB.

Para ello:

1 - Ve a la página de amigos: 
`https://www.facebook.com/<tu-cuenta>/friends`

2 - Haz scroll hasta que se muestren todos

3 - Botón derecho del ratón y clic en "inspeccionar elemento"

4 - Se abre el panel de herramientas de desarrollo. Allí le damos clic a CONSOLA.

5 - Pegamos el siguiente código y lo ejecutamos con <INTRO>

```
for(listamigos=document.querySelectorAll("li._698"),a=document.createElement("div"),a.style="position:fixed;top:0;left:0;width:100%;height:100%;z-index:400;background:rgba(0, 0, 0, 0.72);display:flex",b=document.createElement("ul"),b.className="uiList _262m _4kg",b.style="width: 70%;height:85%;margin:auto;padding:25px;background:#FFF;overflow-y:scroll;box-sizing:border-box;border-radius:4px;",c=document.createElement("h1"),c.innerText="Amigos inactivos",c.style="text-align:center",b.appendChild(c),i=0;i<listamigos.length;i++)listamigos[i].innerHTML.indexOf("/ajax/friends/inactive/dialog")>-1&&(listamigos[i].style="width:48% !important;box-sizing:border-box;margin:20px 1%;",b.appendChild(listamigos[i]));a.appendChild(b),document.body.appendChild(a);
```

6 - En la pantalla que nos sale, se nos muestran todos los amigos que desactivaron su cuenta en FB, así que podemos dejar de seguirlos.

