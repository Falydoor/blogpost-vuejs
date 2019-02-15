<template>
    <div>
        <h2 id="page-heading">
            <span v-text="$t('blogpostApp.operation.home.title')" id="operation-heading">Operations</span>
            <router-link :to="{name: 'OperationCreate'}" tag="button" id="jh-create-entity" class="btn btn-primary float-right jh-create-entity create-operation">
                <font-awesome-icon icon="plus"></font-awesome-icon>
                <span  v-text="$t('blogpostApp.operation.home.createLabel')">
                    Create new Operation
                </span>
            </router-link>
        </h2>
        <b-alert :show="dismissCountDown"
            dismissible
            :variant="alertType"
            @dismissed="dismissCountDown=0"
            @dismiss-count-down="countDownChanged">
            {{alertMessage}}
        </b-alert>
        <br/>
        <div class="table-responsive" v-if="operations">
            <table class="table table-striped">
                <thead>
                <tr>
                    <th><span v-text="$t('global.field.id')">ID</span></th>
                    <th><span v-text="$t('blogpostApp.operation.date')">Date</span></th>
                    <th><span v-text="$t('blogpostApp.operation.description')">Description</span></th>
                    <th><span v-text="$t('blogpostApp.operation.amount')">Amount</span></th>
                    <th><span v-text="$t('blogpostApp.operation.label')">Label</span></th>
                    <th><span v-text="$t('blogpostApp.operation.bankAccount')">Bank Account</span></th>
                    <th></th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="operation in operations"
                    :key="operation.id">
                    <td>
                        <router-link :to="{name: 'OperationView', params: {operationId: operation.id}}">{{operation.id}}</router-link>
                    </td>
                    <td>{{operation.date | formatDate}}</td>
                    <td>{{operation.description}}</td>
                    <td>{{operation.amount}}</td>
                    <td>
                        <span v-for="(label, i) in operation.labels" :key="label.id">{{i > 0 ? ', ' : ''}}
                            <router-link class="form-control-static" :to="{name: 'LabelView', params: {labelId: label.id}}">{{label.label}}</router-link>
                        </span>
                    </td>
                    <td>
                        <div v-if="operation.bankAccount">
                            <router-link :to="{name: 'BankAccountView', params: {bankAccountId: operation.bankAccount.id}}">{{operation.bankAccount.name}}</router-link>
                        </div>
                    </td>
                    <td class="text-right">
                        <div class="btn-group flex-btn-group-container">
                            <router-link :to="{name: 'OperationView', params: {operationId: operation.id}}" tag="button" class="btn btn-info btn-sm">
                                <font-awesome-icon icon="eye"></font-awesome-icon>
                                <span class="d-none d-md-inline" v-text="$t('entity.action.view')">View</span>
                            </router-link>
                            <router-link :to="{name: 'OperationEdit', params: {operationId: operation.id}}"  tag="button" class="btn btn-primary btn-sm">
                                <font-awesome-icon icon="pencil-alt"></font-awesome-icon>
                                <span class="d-none d-md-inline" v-text="$t('entity.action.edit')">Edit</span>
                            </router-link>
                            <b-button v-on:click="prepareRemove(operation)"
                                   class="btn btn-danger btn-sm"
                                   v-b-modal.removeEntity>
                                <font-awesome-icon icon="times"></font-awesome-icon>
                                <span class="d-none d-md-inline" v-text="$t('entity.action.delete')">Delete</span>
                            </b-button>
                        </div>
                    </td>
                </tr>
                </tbody>
            </table>
        </div>
        <b-modal ref="removeEntity" id="removeEntity" >
            <span slot="modal-title"><span id="blogpostApp.operation.delete.question" v-text="$t('entity.delete.title')">Confirm delete operation</span></span>
            <div class="modal-body">
                <p id="jhi-delete-operation-heading" v-bind:title="$t('blogpostApp.operation.delete.question')">Are you sure you want to delete this Operation?</p>
            </div>
            <div slot="modal-footer">
                <button type="button" class="btn btn-secondary" v-text="$t('entity.action.cancel')" v-on:click="closeDialog()">Cancel</button>
                <button type="button" class="btn btn-primary" id="jhi-confirm-delete-operation" v-text="$t('entity.action.delete')" v-on:click="removeOperation()">Delete</button>
            </div>
        </b-modal>
    </div>
</template>

<script lang="ts" src="./operation.component.ts">
</script>
