<script>
import propsBinder from '../utils/propsBinder.js';
import findRealParent from '../utils/findRealParent.js';

const props = {
  url: {
    type: String,
    custom: true
  },
  bounds: {
    custom: true
  },
  opacity: {
    type: Number,
    custom: true,
    default: 1.0
  },
  alt: {
    type: String,
    default: ''
  },
  interactive: {
    type: Boolean,
    default: false
  },
  crossOrigin: {
    type: Boolean,
    default: false
  },
  visible: {
    type: Boolean,
    custom: true,
    default: true
  },
  errorOverlayUrl: {
    type: String,
    custom: true,
    default: ''
  },
  zIndex: {
    type: Number,
    custom: true,
    default: 1
  },
  className: {
    type: String,
    default: ''
  }
};

export default {
  name: 'LImageOverlay',
  props: props,
  mounted () {
    let options = {
      opacity: this.opacity,
      alt: this.alt,
      interactive: this.interactive,
      crossOrigin: this.crossOrigin,
      errorOverlayUrl: this.errorOverlayUrl,
      zIndex: this.zIndex,
      className: this.className
    };
    this.mapObject = L.imageOverlay(this.url, this.bounds, options);
    L.DomEvent.on(this.mapObject, this.$listeners);
    propsBinder(this, this.mapObject, props);
    this.parentContainer = findRealParent(this.$parent);
    this.parentContainer.addLayer(this, !this.visible);
  },
  beforeDestroy () {
    this.parentContainer.removeLayer(this);
  },
  methods: {
    setVisible (newVal, oldVal) {
      if (newVal === oldVal) return;
      if (this.mapObject) {
        if (newVal) {
          this.parentContainer.addLayer(this);
        } else {
          this.parentContainer.removeLayer(this);
        }
      }
    },
    setOpacity (opacity) {
      return this.mapObject.setOpacity(opacity)
    },
    setUrl (url) {
      return this.mapObject.setUrl(url)
    },
    setBounds (bounds) {
      return this.mapObject.setBounds(bounds)
    },
    getBounds () {
      return this.mapObject.getBounds();
    },
    getElement () {
      return this.mapObject.getElement();
    },
    bringToFront () {
      return this.mapObject.bringToFront()
    },
    bringToBack () {
      return this.mapObject.bringToBack()
    },
  },
  render () {
    return null;
  }
};
</script>
