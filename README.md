<script type="text/javascript" defer>
  const rxYoutube = /^.*^((?:https?:)?\/\/)?((?:www|m)\.)?((?:rutube\.ru|rutube.ru))(\/(?:[\w\-]+\?v=|embed\/|v\/|shorts\/)?)([\w\-]+)(\S+)?$/

 const rxScreencast = /^.*^((?:https?:)?\/\/)?(www\.)?(screencast\.com)(\/users)\/([a-z0-9_-]+)\/folders\/([a-z0-9%_-]+)\/media\/([a-z0-9_-]+)(?:\/)?$/im

  window.boot.register('vue', () => {
    window.onload = () => {
      document.querySelectorAll('.contents oembed, .contents a').forEach(elm => {
        const url = elm.hasAttribute('url') ? elm.getAttribute('url') : elm.getAttribute('href')
        let newElmHtml = null
       
        const ytMatch = url.match(rxYoutube)
        const scMatch = url.match(rxScreencast)

 
        if (ytMatch) {
          newElmHtml = `<iframe id="ytplayer" type="text/html" width="640" height="360" src="//rutube.ru/play/embed/{ytMatch[5]}" frameborder="0" allow="accelerometer; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>`
        
      } else if(scMatch) {
        newElmHtml = `<iframe id="scplayer" type="text/html" width="640" height="360" src="${url}/play/embed" frameborder="0" allowfullscreen></iframe>`

        } else if (url.endsWith('.mp4')) {
          newElmHtml = `<video controls autostart="0" name="media" width="640" height="360"><source src="${url}" type="video/mp4"></video>`
        } else {
         return
         }

        const newElm = document.createElement('div') 
        newElm.classList.add('responsive-embed')
        newElm.insertAdjacentHTML('beforeend', newElmHtml)
        elm.replaceWith(newElm)
      })
    }  
  })
</script>
