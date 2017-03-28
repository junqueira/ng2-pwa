"use strict";
var __decorate = (this && this.__decorate) || function (decorators, target, key, desc) {
    var c = arguments.length, r = c < 3 ? target : desc === null ? desc = Object.getOwnPropertyDescriptor(target, key) : desc, d;
    if (typeof Reflect === "object" && typeof Reflect.decorate === "function") r = Reflect.decorate(decorators, target, key, desc);
    else for (var i = decorators.length - 1; i >= 0; i--) if (d = decorators[i]) r = (c < 3 ? d(r) : c > 3 ? d(target, key, r) : d(target, key)) || r;
    return c > 3 && r && Object.defineProperty(target, key, r), r;
};
var __metadata = (this && this.__metadata) || function (k, v) {
    if (typeof Reflect === "object" && typeof Reflect.metadata === "function") return Reflect.metadata(k, v);
};
var core_1 = require('@angular/core');
var enum_1 = require('../enum/enum');
var StorageIntegracaoService = (function () {
    function StorageIntegracaoService() {
        // tslint:disable-next-line:max-line-length
        var store = this.getWebVendasData();
        store.data[String(enum_1.ModuloEnum.LOGIN)] = JSON.parse('{"ip":"10.220.35.46","token":"MDAxMDg0OTg3Nzg0MzQ0MDAwMTIwMDkxMzQ3NyA0ICAwIDAwMDAgMCAgMCAwMDAw","funcionario":{"empresa":21,"matricula":1943774,"nome":"Jose Silva Pedro Legal","apelido":"Josezinho"},"filial":{"empresa":21,"codigo":1000,"atividade":"L","bandeira":1}}');
        sessionStorage['web-vendas'] = JSON.stringify(store);
    }
    /**
     * Método responsável por ler o session storage e, no caso de não haver dados nele,
     * criar uma estrutura de dados vazia.
     */
    StorageIntegracaoService.prototype.getWebVendasData = function () {
        try {
            var store = void 0;
            var defaultValue = { data: {}, triggered: {} };
            store = sessionStorage['web-vendas'];
            if (store == null) {
                store = defaultValue;
            }
            else {
                store = JSON.parse(store);
            }
            return store || defaultValue;
        }
        catch (e) {
            throw new Error('JSON na sessão corrompido');
        }
    };
    /**
     * Método responsável por escrever dados na sessão
     */
    StorageIntegracaoService.prototype.setWebVendasData = function (data) {
        try {
            var store = this.getWebVendasData();
            store.data[String(StorageIntegracaoService.modulo)] = data;
            sessionStorage['web-vendas'] = JSON.stringify(store);
        }
        catch (e) {
            throw new Error('JSON na sessão corrompido');
        }
    };
    StorageIntegracaoService.prototype.on = function (modulo, eventName, calle) {
        //  TODO: escrever lógica de escuta de eventos intramodulares
    };
    StorageIntegracaoService.prototype.trigger = function (eventName, params) {
        //  TODO: escrever lógica de emissão de eventos intramodulares
    };
    StorageIntegracaoService.prototype.read = function (modulo) {
        switch (modulo) {
            case enum_1.ModuloEnum.CATALOGO:
                return this.getDadosCatalogo();
            case enum_1.ModuloEnum.ESTEIRA_PREVENDA:
                return this.getDadosEsteiraDePreVenda();
            case enum_1.ModuloEnum.LOGIN:
                return this.getDadosLogin();
        }
    };
    StorageIntegracaoService.prototype.write = function (data) {
        switch (StorageIntegracaoService.modulo) {
            case enum_1.ModuloEnum.CATALOGO:
                return this.setDadosCatalogo(data);
            case enum_1.ModuloEnum.ESTEIRA_PREVENDA:
                return this.setDadosEsteiraDePreVenda(data);
            case enum_1.ModuloEnum.LOGIN:
                return this.setDadosLogin(data);
        }
    };
    /**
     * Lê os dados contidos no modulo de catalogo
     */
    StorageIntegracaoService.prototype.getDadosCatalogo = function () {
        return null;
    };
    /**
     * Escreve dados para o modulo do catalogo
     */
    StorageIntegracaoService.prototype.setDadosCatalogo = function (data) {
    };
    /**
     * Lê os dados da esteira de pré-venda
     */
    StorageIntegracaoService.prototype.getDadosEsteiraDePreVenda = function () {
        var dadosSession = this.getWebVendasData().data[String(enum_1.ModuloEnum.ESTEIRA_PREVENDA)] || {};
        var infoPreVenda = Object.assign({}, dadosSession);
        infoPreVenda.preVendas = new Map();
        infoPreVenda.imagens = new Map();
        if (!infoPreVenda.cliente) {
            infoPreVenda.cliente = null;
        }
        if (!infoPreVenda.preVendaAtiva) {
            infoPreVenda.preVendaAtiva = null;
        }
        // tslint:disable-next-line:forin
        for (var codigo in dadosSession.preVendas) {
            infoPreVenda.preVendas.set(Number(codigo), dadosSession.preVendas[codigo]);
        }
        // tslint:disable-next-line:forin
        for (var codigoProduto in dadosSession.imagens) {
            infoPreVenda.imagens.set(Number(codigoProduto), dadosSession.imagens[codigoProduto]);
        }
        return infoPreVenda;
    };
    /**
     * Persiste dados para o modulo de pré-vendas
     */
    StorageIntegracaoService.prototype.setDadosEsteiraDePreVenda = function (data) {
        var mePersiste = {};
        Object.assign(mePersiste, data);
        mePersiste.preVendas = {};
        mePersiste.imagens = {};
        data.preVendas.forEach(function (value, key) {
            mePersiste.preVendas[key] = value;
        });
        data.imagens.forEach(function (value, key) {
            mePersiste.imagens[key] = value;
        });
        this.setWebVendasData(mePersiste);
    };
    /**
     * Lê os dados contidos no modulo de login
     */
    StorageIntegracaoService.prototype.getDadosLogin = function () {
        return this.getWebVendasData().data[String(enum_1.ModuloEnum.LOGIN)];
    };
    /**
     * Lê os dados do modulo de login
     */
    StorageIntegracaoService.prototype.setDadosLogin = function (data) {
        this.setWebVendasData(data);
    };
    StorageIntegracaoService = __decorate([
        core_1.Injectable(), 
        __metadata('design:paramtypes', [])
    ], StorageIntegracaoService);
    return StorageIntegracaoService;
}());
exports.StorageIntegracaoService = StorageIntegracaoService;
