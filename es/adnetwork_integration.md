---
"$title": Integración con AMP para publicar anuncios gráficos
"$order": '5'
description: Esta guía está destinada a las redes publicitarias que desean integrarse con AMP para publicar anuncios gráficos en páginas AMP.
formats:
  - anuncios
---

Esta guía está destinada a las redes publicitarias que desean integrarse con AMP para publicar anuncios gráficos en páginas AMP.

## Descripción general

Como servidor de anuncios, puede integrarse con AMP para publicar anuncios HTML tradicionales en páginas AMP, además de [publicar](../../../documentation/guides-and-tutorials/learn/intro-to-amphtml-ads.md) anuncios AMP HTML.

##### ¿Quiere publicar anuncios HTML tradicionales?

1. [`amp-ad`](../../../documentation/components/reference/amp-ad.md)

##### ¿Quiere publicar anuncios HTML de AMP?

1. [`amp-ad`](../../../documentation/components/reference/amp-ad.md) (es decir, si aún no ha creado uno para publicar anuncios HTML tradicionales).
2.  [Cree una integración Fast Fetch para publicar anuncios HTML de AMP](https://www.onliner.by/) .

## Crear un `amp-ad`<a name="creating-an-amp-ad"></a>

Como servidor de anuncios, los editores a los que da soporte incluyen una biblioteca de JavaScript proporcionada por usted y colocan varios "fragmentos de anuncios" que dependen de la biblioteca de JavaScript para buscar anuncios y representarlos en el sitio web del editor. Debido a que AMP no permite que los editores ejecuten JavaScript arbitrario, deberá contribuir al código de fuente abierta de AMP para permitir que la [`amp-ad`](../../../documentation/components/reference/amp-ad.md) solicite anuncios de su servidor de anuncios.

[tip type="note"] **NOTA:** puede utilizar esta [`amp-ad`](../../../documentation/components/reference/amp-ad.md) para mostrar anuncios HTML tradicionales **y** anuncios AMP HTML. [/tip]

Por ejemplo, el servidor de Amazon A9 se puede invocar utilizando la siguiente sintaxis:

```html
<amp-ad width="300" height="250"
    type="a9"
    data-aax_size="300x250"
    data-aax_pubname="test123"
    data-aax_src="302">
</amp-ad>
```

En el código anterior, el `type` especifica la red publicitaria, que en este caso es A9. Los `data-*` dependen de los parámetros que el servidor A9 de Amazon espera entregar un anuncio. El [`a9.js`](https://github.com/ampproject/amphtml/blob/master/ads/a9.js) muestra cómo se asignan los parámetros para realizar una llamada de JavaScript a la URL del servidor A9. Los parámetros correspondientes transmitidos por la [`amp-ad`](../../../documentation/components/reference/amp-ad.md) se añaden a la URL para devolver un anuncio.

Para obtener instrucciones sobre cómo crear una integración de [`amp-ad`](../../../documentation/components/reference/amp-ad.md) [, consulte Integración de redes publicitarias en AMP](https://github.com/ampproject/amphtml/blob/master/ads/README.md) .

## Crear una integración Fast Fetch<a name="creating-a-fast-fetch-integration"></a>

[Fast Fetch](https://blog.amp.dev/2017/08/21/even-faster-loading-ads-in-amp/) es un mecanismo de AMP que separa la solicitud de anuncio de la respuesta al anuncio, lo que permite que las solicitudes de anuncios se produzcan antes en el ciclo de vida de la página y mostrar anuncios solo cuando es probable que los usuarios los vean. Fast Fetch proporciona un trato preferencial a los anuncios HTML de AMP verificados sobre los anuncios HTML tradicionales. En Fast Fetch, si un anuncio falla en la validación, ese anuncio se envuelve en un iframe entre dominios para protegerlo del resto del documento AMP. Por el contrario, un anuncio HTML de AMP que pasa la validación se escribe directamente en la página. Fast Fetch maneja anuncios AMP y no AMP; no se requieren solicitudes de anuncios adicionales para los anuncios que no superan la validación.

{{image ('/ static / img / docs / ads / amphtml-ad-flow.svg', 843, 699, alt = 'Flujo de integración de búsqueda rápida', caption = 'Flujo de integración de búsqueda rápida')}}

Para publicar anuncios HTML de AMP desde su servidor de anuncios, debe proporcionar una integración de Búsqueda rápida que incluya:

1. Compatible con la comunicación de red SSL.
2. Proporcionar JavaScript para crear la solicitud de anuncio (implementaciones de ejemplo: [AdSense](https://github.com/ampproject/amphtml/tree/master/extensions/amp-ad-network-adsense-impl) y [DoubleClick](https://github.com/ampproject/amphtml/tree/master/extensions/amp-ad-network-doubleclick-impl) ).
3. Validar y firmar la creatividad a través de un servicio de validación. [Cloudflare](https://blog.cloudflare.com/firebolt/) proporciona un servicio de verificación de anuncios AMP, que permite a cualquier proveedor de anuncios independiente ofrecer anuncios más rápidos, ligeros y atractivos.

Para obtener instrucciones sobre cómo crear una integración de Búsqueda rápida, consulte la [Guía de implementación de red de Búsqueda rápida](https://github.com/ampproject/amphtml/blob/master/ads/google/a4a/docs/Network-Impl-Guide.md) .

## Recursos Relacionados

- [`amp-ad`](../../../documentation/components/reference/amp-ad.md)
- [Lista de proveedores de publicidad admitidos](../../../documentation/guides-and-tutorials/develop/monetization/ads_vendors.md)
- [Entrada de blog que describe el lanzamiento de Fast Fetch](https://blog.amp.dev/2017/08/21/even-faster-loading-ads-in-amp/)
