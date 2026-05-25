class MiAsistente extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    
    this.abierto = false;
    this.esquina = 'bottom-right';
    
    // Opciones que se mostrarán en la grilla (luego vendrán de tu API)
    this.opciones = [
      { nombre: 'Sistema Académico', url: 'https://ejemplo.com/academico', icono: '🎓', color: '#0078d4' },
      { nombre: 'Biblioteca',        url: 'https://ejemplo.com/biblioteca', icono: '📚', color: '#00bcf2' },
      { nombre: 'Correo',            url: 'https://outlook.com', icono: '✉️', color: '#0078d4' },
      { nombre: 'Drive',             url: 'https://drive.google.com', icono: '☁️', color: '#1ba1e2' },
      { nombre: 'Calendario',        url: 'https://calendar.google.com', icono: '📅', color: '#d83b01' },
      { nombre: 'Documentos',        url: 'https://docs.google.com', icono: '📄', color: '#2b579a' },
      { nombre: 'Hojas',             url: 'https://sheets.google.com', icono: '📊', color: '#217346' },
      { nombre: 'Presentaciones',    url: 'https://slides.google.com', icono: '🎯', color: '#b7472a' },
      { nombre: 'Notas',             url: 'https://keep.google.com', icono: '📝', color: '#7719aa' },
    ];
  }

  connectedCallback() {
    this.render();
  }

  render() {
    const posiciones = {
      'bottom-right': 'bottom: 20px; right: 20px;',
      'bottom-left':  'bottom: 20px; left: 20px;',
      'top-right':    'top: 20px; right: 20px;',
      'top-left':     'top: 20px; left: 20px;',
    };

    const esArriba = this.esquina.startsWith('top');
    const esDerecha = this.esquina.endsWith('right');

    // Posición del modal relativa al botón
    const modalPos = {
      'bottom-right': 'bottom: 75px; right: 0;',
      'bottom-left':  'bottom: 75px; left: 0;',
      'top-right':    'top: 75px; right: 0;',
      'top-left':     'top: 75px; left: 0;',
    };

    this.shadowRoot.innerHTML = `
      <style>
        :host {
          position: fixed;
          ${posiciones[this.esquina]}
          z-index: 9999;
          font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
        }

        .contenedor {
          position: relative;
        }

        .boton-principal {
          width: 56px;
          height: 56px;
          border-radius: 50%;
          background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
          border: none;
          cursor: pointer;
          box-shadow: 0 4px 14px rgba(0,0,0,0.25);
          color: white;
          transition: transform 0.2s, box-shadow 0.2s;
          display: flex;
          align-items: center;
          justify-content: center;
          padding: 0;
        }

        .boton-principal:hover {
          transform: scale(1.08);
          box-shadow: 0 6px 18px rgba(0,0,0,0.3);
        }

        .boton-principal svg {
          width: 24px;
          height: 24px;
        }

        /* Modal estilo Microsoft 365 */
        .modal {
          display: ${this.abierto ? 'block' : 'none'};
          position: absolute;
          ${modalPos[this.esquina]}
          width: 360px;
          background: white;
          border-radius: 8px;
          box-shadow: 0 8px 24px rgba(0,0,0,0.2);
          padding: 20px;
          animation: aparecer 0.18s ease-out;
        }

        @keyframes aparecer {
          from { 
            opacity: 0; 
            transform: translateY(${esArriba ? '-8px' : '8px'}) scale(0.96); 
          }
          to { 
            opacity: 1; 
            transform: translateY(0) scale(1); 
          }
        }

        .modal-header {
          font-size: 13px;
          color: #605e5c;
          font-weight: 600;
          margin-bottom: 14px;
          padding-bottom: 10px;
          border-bottom: 1px solid #edebe9;
        }

        .grilla {
          display: grid;
          grid-template-columns: repeat(3, 1fr);
          gap: 4px;
        }

        .opcion {
          display: flex;
          flex-direction: column;
          align-items: center;
          padding: 12px 8px;
          border-radius: 6px;
          text-decoration: none;
          color: #323130;
          cursor: pointer;
          transition: background 0.15s;
          text-align: center;
        }

        .opcion:hover {
          background: #f3f2f1;
        }

        .opcion-icono {
          width: 40px;
          height: 40px;
          border-radius: 8px;
          display: flex;
          align-items: center;
          justify-content: center;
          font-size: 22px;
          margin-bottom: 6px;
          background: var(--color-icono);
          color: white;
        }

        .opcion-nombre {
          font-size: 11px;
          line-height: 1.3;
          word-break: break-word;
          max-width: 100%;
        }

        /* Footer con selector de esquinas */
        .modal-footer {
          margin-top: 14px;
          padding-top: 10px;
          border-top: 1px solid #edebe9;
        }

        .footer-label {
          font-size: 11px;
          color: #605e5c;
          margin-bottom: 6px;
        }

        .esquinas {
          display: grid;
          grid-template-columns: repeat(4, 1fr);
          gap: 4px;
        }

        .esquinas button {
          padding: 6px;
          border: 1px solid #d2d0ce;
          background: white;
          border-radius: 4px;
          cursor: pointer;
          font-size: 14px;
          transition: all 0.15s;
        }

        .esquinas button:hover {
          background: #f3f2f1;
        }

        .esquinas button.actual {
          background: #0078d4;
          color: white;
          border-color: #0078d4;
        }
      </style>

      <div class="contenedor">
        <button class="boton-principal" id="btn-principal" aria-label="Menú">
          <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M3 6h18M3 12h18M3 18h18" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"/>
          </svg>
        </button>

        <div class="modal">
          <div class="modal-header">Aplicaciones</div>
          
          <div class="grilla">
            ${this.opciones.map(op => `
              <a class="opcion" href="${op.url}" target="_blank" rel="noopener noreferrer">
                <div class="opcion-icono" style="--color-icono: ${op.color}">
                  ${op.icono}
                </div>
                <div class="opcion-nombre">${op.nombre}</div>
              </a>
            `).join('')}
          </div>

          <div class="modal-footer">
            <div class="footer-label">Posición del botón</div>
            <div class="esquinas">
              <button data-esquina="top-left"     class="${this.esquina === 'top-left' ? 'actual' : ''}" title="Arriba izquierda">↖</button>
              <button data-esquina="top-right"    class="${this.esquina === 'top-right' ? 'actual' : ''}" title="Arriba derecha">↗</button>
              <button data-esquina="bottom-left"  class="${this.esquina === 'bottom-left' ? 'actual' : ''}" title="Abajo izquierda">↙</button>
              <button data-esquina="bottom-right" class="${this.esquina === 'bottom-right' ? 'actual' : ''}" title="Abajo derecha">↘</button>
            </div>
          </div>
        </div>
      </div>
    `;

    // Toggle modal
    this.shadowRoot.getElementById('btn-principal').addEventListener('click', (e) => {
      e.stopPropagation();
      this.abierto = !this.abierto;
      this.render();
    });

    // Cambiar esquina
    this.shadowRoot.querySelectorAll('.esquinas button').forEach(btn => {
      btn.addEventListener('click', (e) => {
        e.stopPropagation();
        this.esquina = btn.dataset.esquina;
        this.render();
      });
    });

    // Cerrar al hacer clic fuera
    if (this.abierto) {
      this._cerrarFuera = (e) => {
        if (!this.contains(e.target)) {
          this.abierto = false;
          this.render();
        }
      };
      setTimeout(() => document.addEventListener('click', this._cerrarFuera), 0);
    } else if (this._cerrarFuera) {
      document.removeEventListener('click', this._cerrarFuera);
    }
  }
}

customElements.define('mi-asistente', MiAsistente);
