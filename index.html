// ==================== SERVICE WORKER (PWA) ====================
if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
        navigator.serviceWorker.register('sw.js')
            .then(reg => console.log('✅ Service Worker registrado'))
            .catch(err => console.log('❌ Erro SW:', err));
    });
}

// ==================== VARIÁVEIS GLOBAIS ====================
let base = JSON.parse(localStorage.getItem('base')) || null;
let destinos = JSON.parse(localStorage.getItem('destinos')) || [];
let viagens = JSON.parse(localStorage.getItem('viagens')) || [];
let sequenciaAtual = [];
let map, rotaLayer, baseMap;
let posicaoGPSAtual = null;
const cacheCoordenadas = JSON.parse(localStorage.getItem('cacheCoords')) || {};

// ==================== INICIALIZAÇÃO ====================
document.addEventListener('DOMContentLoaded', function() {
    inicializarBase();
    atualizarListaDestinos();
    atualizarBotoesDestinos();
    atualizarHistorico();
    atualizarEstatisticas();
    initMap();
    verificarInstalacao();
});

// ==================== PWA - INSTALAÇÃO ====================
let deferredPrompt;

window.addEventListener('beforeinstallprompt', (e) => {
    e.preventDefault();
    deferredPrompt = e;

    // Mostrar prompt customizado após 3s
    setTimeout(() => {
        document.getElementById('installPrompt').classList.add('show');
    }, 3000);
});

function instalarApp() {
    if (deferredPrompt) {
        deferredPrompt.prompt();
        deferredPrompt.userChoice.then((choiceResult) => {
            if (choiceResult.outcome === 'accepted') {
                console.log('App instalado!');
            }
            deferredPrompt = null;
            fecharPrompt();
        });
    } else {
        // iOS - instruções manuais
        alert('📱 Para instalar no iPhone:\n\n1. Toque no ícone de compartilhar (quadrado com seta)\n2. Role e toque em "Adicionar à Tela de Início"\n3. Toque em "Adicionar"');
        fecharPrompt();
    }
}

function fecharPrompt() {
    document.getElementById('installPrompt').classList.remove('show');
}

function verificarInstalacao() {
    if (window.matchMedia('(display-mode: standalone)').matches) {
        console.log('✅ App rodando como PWA');
    }
}

// ==================== NAVEGAÇÃO ====================
function switchTab(tabName) {
    document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
    document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));

    event.target.classList.add('active');
    document.getElementById(tabName).classList.add('active');

    if (tabName === 'viagem' && (base || posicaoGPSAtual) && sequenciaAtual.length > 0) {
        desenharRota();
    } else if (tabName === 'relatorio') {
        atualizarEstatisticas();
    }
}

// ==================== GPS ====================
async function usarGPSAtual() {
    const btnGPS = document.getElementById('btnGPS');
    btnGPS.disabled = true;
    btnGPS.innerHTML = '<span class="loading"></span> Buscando GPS...';

    if (!navigator.geolocation) {
        alert('GPS não suportado. Use base fixa.');
        btnGPS.disabled = false;
        btnGPS.innerHTML = '📍 Usar GPS Atual';
        return;
    }

    navigator.geolocation.getCurrentPosition(
        (position) => {
            posicaoGPSAtual = {
                lat: position.coords.latitude,
                lng: position.coords.longitude,
                nome: 'GPS Atual',
                endereco: 'Localização detectada'
            };

            document.getElementById('gpsInfo').style.display = 'block';
            document.getElementById('gpsCoords').textContent = `${posicaoGPSAtual.lat.toFixed(6)}, ${posicaoGPSAtual.lng.toFixed(6)}`;

            if (map) {
                map.setView([posicaoGPSAtual.lat, posicaoGPSAtual.lng], 15);
                L.marker([posicaoGPSAtual.lat, posicaoGPSAtual.lng]).addTo(map)
                    .bindPopup('<b>Você está aqui</b>')
                    .openPopup();
            }

            alert('✅ GPS capturado!');
            btnGPS.disabled = false;
            btnGPS.innerHTML = '✅ GPS Ativo';
        },
        (error) => {
            let msg = 'Erro GPS: ';
            switch(error.code) {
                case error.PERMISSION_DENIED:
                    msg += 'Permissão negada. Vá em Configurações > Safari > Localização.';
                    break;
                case error.POSITION_UNAVAILABLE:
                    msg += 'Localização indisponível.';
                    break;
                default:
                    msg += 'Erro desconhecido.';
            }
            alert(msg);
            btnGPS.disabled = false;
            btnGPS.innerHTML = '📍 Usar GPS Atual';
        },
        {
            enableHighAccuracy: true,
            timeout: 10000,
            maximumAge: 60000
        }
    );
}

// ==================== BASE ====================
function inicializarBase() {
    if (base) {
        document.getElementById('enderecoBase').value = base.endereco;
        document.getElementById('infoBase').style.display = 'block';
        document.getElementById('enderecoBaseDisplay').textContent = base.endereco;
        document.getElementById('coordsBase').textContent = `${base.lat.toFixed(6)}, ${base.lng.toFixed(6)}`;
        document.getElementById('btnBaseText').textContent = 'Atualizar';
        initBaseMap();
    }
}

async function configurarBase() {
    const endereco = document.getElementById('enderecoBase').value.trim();

    if (!endereco) {
        alert('Digite seu endereço.');
        return;
    }

    document.getElementById('btnBaseText').style.display = 'none';
    document.getElementById('btnBaseLoading').style.display = 'inline-block';

    try {
        const coords = await buscarCoordenadas(endereco, true);
        if (!coords) {
            alert('Endereço não encontrado.');
            return;
        }

        base = {
            id: 'base',
            endereco,
            lat: coords.lat,
            lng: coords.lng,
            nome: 'Base Fixa'
        };

        localStorage.setItem('base', JSON.stringify(base));
        cacheCoordenadas[endereco.toLowerCase()] = coords;
        localStorage.setItem('cacheCoords', JSON.stringify(cacheCoordenadas));

        inicializarBase();
        alert('✅ Base salva!');

    } catch (error) {
        alert('Erro ao salvar base.');
    } finally {
        document.getElementById('btnBaseText').style.display = 'inline';
        document.getElementById('btnBaseLoading').style.display = 'none';
    }
}

function limparBase() {
    if (confirm('Alterar base?')) {
        localStorage.removeItem('base');
        base = null;
        document.getElementById('infoBase').style.display = 'none';
        document.getElementById('mapaBase').style.display = 'none';
        document.getElementById('enderecoBase').value = '';
        document.getElementById('btnBaseText').textContent = 'Salvar';
    }
}

function initBaseMap() {
    if (!base) return;

    baseMap = L.map('mapaBase').setView([base.lat, base.lng], 15);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(baseMap);

    L.marker([base.lat, base.lng]).addTo(baseMap)
        .bindPopup(`<b>Base</b><br>${base.endereco}`)
        .openPopup();

    document.getElementById('mapaBase').style.display = 'block';
}

// ==================== GEOCODING ====================
async function buscarCoordenadas(endereco, isBase = false) {
    const cacheKey = endereco.toLowerCase();

    if (cacheCoordenadas[cacheKey]) {
        return cacheCoordenadas[cacheKey];
    }

    const url = `https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(endereco)}&limit=1&countrycodes=br`;

    try {
        const response = await fetch(url, {
            headers: { 'User-Agent': 'ControleKM-PWA/1.0' }
        });

        const data = await response.json();

        if (data && data.length > 0) {
            const coords = { lat: parseFloat(data[0].lat), lng: parseFloat(data[0].lon) };
            cacheCoordenadas[cacheKey] = coords;
            localStorage.setItem('cacheCoords', JSON.stringify(cacheCoordenadas));
            return coords;
        }
        return null;
    } catch (error) {
        console.error('Erro geocoding:', error);
        return null;
    }
}

// ==================== DESTINOS ====================
async function adicionarDestino() {
    const nome = document.getElementById('nomeDestino').value.trim();
    const endereco = document.getElementById('enderecoDestino').value.trim();

    if (!nome || !endereco) {
        alert('Preencha nome e endereço.');
        return;
    }

    if (!base && !posicaoGPSAtual) {
        alert('Configure base ou use GPS primeiro.');
        return;
    }

    document.getElementById('btnDestinoText').style.display = 'none';
    document.getElementById('btnDestinoLoading').style.display = 'inline-block';

    try {
        const coords = await buscarCoordenadas(endereco);
        if (!coords) {
            alert('Endereço não encontrado.');
            return;
        }

        const novoDestino = {
            id: Date.now(),
            nome,
            endereco,
            lat: coords.lat,
            lng: coords.lng
        };

        destinos.push(novoDestino);
        salvarDados();

        document.getElementById('nomeDestino').value = '';
        document.getElementById('enderecoDestino').value = '';

        atualizarListaDestinos();
        atualizarBotoesDestinos();

        alert(`✅ Agência "${nome}" adicionada!`);

    } catch (error) {
        alert('Erro ao adicionar agência.');
    } finally {
        document.getElementById('btnDestinoText').style.display = 'inline';
        document.getElementById('btnDestinoLoading').style.display = 'none';
    }
}

function atualizarListaDestinos() {
    const lista = document.getElementById('listaDestinos');

    if (destinos.length === 0) {
        lista.innerHTML = `
            <div class="empty-state">
                <div class="empty-state-icon">📍</div>
                <p>Nenhuma agência cadastrada</p>
            </div>
        `;
        return;
    }

    lista.innerHTML = destinos.map(destino => `
        <div class="destino-item">
            <div class="destino-name">${destino.nome}</div>
            <div class="destino-address">${destino.endereco}</div>
            <button class="btn btn-danger" onclick="removerDestino(${destino.id})" style="margin-top: 8px;">
                🗑️ Remover
            </button>
        </div>
    `).join('');
}

function removerDestino(id) {
    if (confirm('Remover esta agência?')) {
        destinos = destinos.filter(d => d.id !== id);
        salvarDados();
        atualizarListaDestinos();
        atualizarBotoesDestinos();
    }
}

// ==================== VIAGEM ====================
function atualizarBotoesDestinos() {
    const container = document.getElementById('botoesDestinos');

    if (!base && !posicaoGPSAtual) {
        container.innerHTML = `
            <div class="empty-state">
                <div class="empty-state-icon">⚠️</div>
                <p>Configure base ou use GPS</p>
            </div>
        `;
        return;
    }

    if (destinos.length === 0) {
        container.innerHTML = `
            <div class="empty-state">
                <div class="empty-state-icon">📍</div>
                <p>Cadastre agências primeiro</p>
            </div>
        `;
        return;
    }

    container.innerHTML = destinos.map(destino => {
        const selecionado = sequenciaAtual.some(s => s.id === destino.id);
        return `
            <button class="destino-btn ${selecionado ? 'selected' : ''}" onclick="adicionarNaSequencia(${destino.id})">
                <div class="destino-name">${destino.nome}</div>
                <div class="destino-address">${destino.endereco.split(',')[0]}</div>
            </button>
        `;
    }).join('');
}

function adicionarNaSequencia(id) {
    const destino = destinos.find(d => d.id === id);
    if (sequenciaAtual.some(s => s.id === id)) {
        alert('Agência já está na rota!');
        return;
    }

    sequenciaAtual.push(destino);
    atualizarSequencia();
    atualizarBotoesDestinos();
}

function atualizarSequencia() {
    const container = document.getElementById('sequenciaViagem');
    const totalElement = document.getElementById('totalKm');

    if (sequenciaAtual.length === 0) {
        container.innerHTML = `
            <div style="color: #a0aec0; text-align: center; padding: 15px; font-size: 14px;">
                Clique nas agências para montar a rota
            </div>
        `;
        totalElement.textContent = '0 km';
        document.getElementById('btnSalvar').disabled = true;
        return;
    }

    container.innerHTML = sequenciaAtual.map((destino, index) => `
        <div class="sequencia-item">
            <div class="sequencia-number">${index + 1}</div>
            <div style="flex: 1;">
                <div style="font-weight: 500; font-size: 14px;">${destino.nome}</div>
                <small style="color: #666; font-size: 12px;">${destino.endereco.split(',').slice(0,2).join(',')}</small>
            </div>
            <button class="btn btn-danger" onclick="removerDaSequencia(${index})" style="padding: 6px 10px; font-size: 12px;">×</button>
        </div>
    `).join('');

    desenharRota();
}

function removerDaSequencia(index) {
    sequenciaAtual.splice(index, 1);
    atualizarSequencia();
    atualizarBotoesDestinos();
}

function limparSequencia() {
    sequenciaAtual = [];
    posicaoGPSAtual = null;
    document.getElementById('gpsInfo').style.display = 'none';
    document.getElementById('btnGPS').innerHTML = '📍 Usar GPS Atual';
    atualizarSequencia();
    atualizarBotoesDestinos();
}

// ==================== MAPA E ROTA ====================
function initMap() {
    map = L.map('map').setView([-29.94, -50.99], 12);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap',
        maxZoom: 19
    }).addTo(map);

    if (base) {
        L.marker([base.lat, base.lng]).addTo(map).bindPopup('Base Fixa');
    }
}

async function desenharRota() {
    if (rotaLayer) {
        map.removeLayer(rotaLayer);
    }

    const statusDiv = document.getElementById('rotaInfo');
    const totalElement = document.getElementById('totalKm');

    if (sequenciaAtual.length === 0) {
        statusDiv.style.display = 'none';
        totalElement.textContent = '0 km';
        map.setView([-29.94, -50.99], 12);
        return;
    }

    statusDiv.style.display = 'block';
    totalElement.textContent = 'Calculando...';

    try {
        const pontoInicial = posicaoGPSAtual || base;
        if (!pontoInicial) {
            alert('Configure base ou use GPS');
            return;
        }

        const pontosRota = [pontoInicial, ...sequenciaAtual, pontoInicial];
        const coordenadas = pontosRota.map(p => `${p.lng},${p.lat}`).join(';');

        const url = `https://router.project-osrm.org/route/v1/driving/${coordenadas}?overview=full&geometries=geojson`;

        const response = await fetch(url);
        const data = await response.json();

        if (data.code === 'Ok' && data.routes.length > 0) {
            const rota = data.routes[0];
            const distanciaKm = (rota.distance / 1000).toFixed(2);
            const tempoMinutos = Math.round(rota.duration / 60);

            rotaLayer = L.geoJSON(rota.geometry, {
                style: {
                    color: '#e74c3c',
                    weight: 5,
                    opacity: 0.8
                }
            }).addTo(map);

            pontosRota.forEach((ponto, index) => {
                const icone = index === 0 || index === pontosRota.length - 1 ? '🏠' : index;

                L.marker([ponto.lat, ponto.lng], {
                    icon: L.divIcon({
                        html: icone,
                        iconSize: [24, 24],
                        className: 'custom-marker'
                    })
                }).addTo(map).bindPopup(ponto.nome || ponto.endereco);
            });

            map.fitBounds(rotaLayer.getBounds().pad(0.1));

            totalElement.innerHTML = `${distanciaKm} km <small>(~${tempoMinutos} min)</small>`;
            statusDiv.style.display = 'none';
            document.getElementById('btnSalvar').disabled = false;

        } else {
            throw new Error('Rota não encontrada');
        }
    } catch (error) {
        console.error('Erro rota:', error);
        totalElement.textContent = 'Erro';
        statusDiv.style.display = 'none';
        alert('Erro ao calcular rota. Verifique conexão.');
    }
}

async function salvarViagem() {
    if (sequenciaAtual.length === 0) {
        alert('Adicione agências à rota!');
        return;
    }

    const kmTotal = parseFloat(document.getElementById('totalKm').textContent);
    if (isNaN(kmTotal) || kmTotal <= 0) {
        alert('Calcule a rota primeiro!');
        return;
    }

    const viagem = {
        id: Date.now(),
        data: new Date().toISOString(),
        base: posicaoGPSAtual ? 'GPS Atual' : base.endereco,
        destinos: sequenciaAtual.map(d => d.nome),
        kmTotal
    };

    viagens.push(viagem);
    salvarDados();

    alert(`✅ Viagem salva!\n\n📏 ${kmTotal.toFixed(2)} km\n📍 ${viagem.destinos.join(' → ')}`);

    limparSequencia();
    atualizarHistorico();
    atualizarEstatisticas();
}

// ==================== HISTÓRICO ====================
function atualizarHistorico() {
    const lista = document.getElementById('listaHistorico');

    if (viagens.length === 0) {
        lista.innerHTML = `
            <div class="empty-state">
                <div class="empty-state-icon">📋</div>
                <p>Nenhuma viagem registrada</p>
            </div>
        `;
        return;
    }

    const viagensOrdenadas = [...viagens].sort((a, b) => new Date(b.data) - new Date(a.data));

    lista.innerHTML = viagensOrdenadas.map(viagem => {
        const data = new Date(viagem.data);
        const dataFormatada = data.toLocaleDateString('pt-BR') + ' ' + 
            data.toLocaleTimeString('pt-BR', {hour: '2-digit', minute: '2-digit'});

        return `
            <div class="historico-item">
                <div class="historico-date">${dataFormatada}</div>
                <div class="historico-route">
                    🏠 ${viagem.base} → ${viagem.destinos.join(' → ')} → 🏠
                </div>
                <div class="historico-km">📏 ${viagem.kmTotal.toFixed(2)} km</div>
            </div>
        `;
    }).join('');
}

// ==================== ESTATÍSTICAS ====================
function atualizarEstatisticas() {
    const hoje = new Date().toDateString();
    const mesAtual = new Date().getMonth();
    const anoAtual = new Date().getFullYear();

    const kmHoje = viagens
        .filter(v => new Date(v.data).toDateString() === hoje)
        .reduce((sum, v) => sum + v.kmTotal, 0);

    const kmMes = viagens
        .filter(v => {
            const data = new Date(v.data);
            return data.getMonth() === mesAtual && data.getFullYear() === anoAtual;
        })
        .reduce((sum, v) => sum + v.kmTotal, 0);

    const kmTotal = viagens.reduce((sum, v) => sum + v.kmTotal, 0);

    document.getElementById('statHoje').textContent = kmHoje.toFixed(1);
    document.getElementById('statMes').textContent = kmMes.toFixed(1);
    document.getElementById('statTotal').textContent = kmTotal.toFixed(1);
}

// ==================== EXPORTAÇÃO ====================
function exportarCSV() {
    if (viagens.length === 0) {
        alert('Nenhuma viagem para exportar!');
        return;
    }

    let csv = 'Data,Hora,Base,Rota,Quilometragem\n';

    viagens.forEach(v => {
        const data = new Date(v.data);
        const dataStr = data.toLocaleDateString('pt-BR');
        const horaStr = data.toLocaleTimeString('pt-BR', {hour: '2-digit', minute: '2-digit'});
        const rota = v.destinos.join(' → ');
        csv += `"${dataStr}","${horaStr}","${v.base}","${rota}","${v.kmTotal.toFixed(2)}"\n`;
    });

    downloadArquivo(csv, `ressarcimento_${new Date().toISOString().split('T')[0]}.csv`, 'text/csv');
}

function exportarTXT() {
    if (viagens.length === 0) {
        alert('Nenhuma viagem para exportar!');
        return;
    }

    let texto = '=== RELATÓRIO DE QUILOMETRAGEM ===\n\n';

    viagens.forEach(v => {
        const data = new Date(v.data);
        texto += `${data.toLocaleDateString('pt-BR')} ${data.toLocaleTimeString('pt-BR', {hour: '2-digit', minute: '2-digit'})}\n`;
        texto += `Base: ${v.base}\n`;
        texto += `Rota: ${v.destinos.join(' → ')}\n`;
        texto += `KM: ${v.kmTotal.toFixed(2)}\n\n`;
    });

    const total = viagens.reduce((sum, v) => sum + v.kmTotal, 0);
    texto += `\nTOTAL: ${total.toFixed(2)} km`;

    downloadArquivo(texto, `ressarcimento_${new Date().toISOString().split('T')[0]}.txt`, 'text/plain');
}

function downloadArquivo(conteudo, nomeArquivo, tipo) {
    const blob = new Blob([conteudo], {type: tipo + ';charset=utf-8;'});
    const url = URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.href = url;
    link.download = nomeArquivo;
    link.click();
    URL.revokeObjectURL(url);
}

// ==================== SALVAR DADOS ====================
function salvarDados() {
    localStorage.setItem('destinos', JSON.stringify(destinos));
    localStorage.setItem('viagens', JSON.stringify(viagens));
}

function limparHistorico() {
    if (confirm('⚠️ Limpar TODO o histórico? Esta ação não pode ser desfeita!')) {
        viagens = [];
        localStorage.setItem('viagens', JSON.stringify(viagens));
        atualizarHistorico();
        atualizarEstatisticas();
        alert('✅ Histórico limpo!');
    }
}
