<template>
    <v-container>
        <v-row >
            <v-col cols='3'>
                <v-text-field
                type='number'
                label="요금 입력"
                v-model="fee">
                </v-text-field>
            </v-col>
            <v-col cols='3'>
                <v-text-field
                type='number'
                label="요율 입력"
                v-model="rate">
                </v-text-field>
            </v-col>
            <v-col cols='3'>
                <v-text-field
                label="보험 기간 입력  (형식 : mm/dd/yyyy)"
                v-model="period">
                </v-text-field>
            </v-col>
            <v-col cols='3'>
                <v-text-field
                type='number'
                label="최대 보상 금액"
                v-model="maximumReward">
                </v-text-field>
            </v-col>
        </v-row>
        <v-row >
            <v-col cols='12'>
                <v-text-field
                label="제한 사항 입력"
                v-model="restriction">
                </v-text-field>
            </v-col>
        </v-row>
        <Editor :initialValue="editorText" ref="toastEditor" initialEditType="wysiwyg"  height="500px" />
        
        
        <v-row >
            <v-col cols='3'>
                <v-text-field
                type='number'
                label="사업자 번호"
                v-model="businessNo">
                </v-text-field>
            </v-col>
            <v-col cols='3'>
                <v-text-field
                type='number'
                label="안전등급"
                v-model="safeGrade">
                </v-text-field>
            </v-col>
            <v-col cols='3'>
                <v-text-field
                label="직장 유형"
                v-model="workplaceType">
                </v-text-field>
            </v-col>
            
        </v-row>
        <v-row >
            <v-col cols='3'>
                <v-text-field
                label="직장내 직위"
                v-model="workplacePosition">
                </v-text-field>
            </v-col>
            <v-col cols='6'>
                <v-text-field
                label="직장 주소"
                v-model="workplaceAddress">
                </v-text-field>
            </v-col>
        </v-row>
        <v-row justify='center'>
            <v-col cols='4'>
                <v-btn  block elvation="2" v-on:click="save">
                    보험 추가하기
                </v-btn>
            </v-col>
            <v-col cols='4'>
                <v-btn  block elvation="2" v-on:click="goIdeaPage">
                    보험 취소하기
                </v-btn>
            </v-col>
        </v-row>
    </v-container>
    
</template>
<script>
import 'codemirror/lib/codemirror.css'; 
import '@toast-ui/editor/dist/toastui-editor.css'; 
import { Editor } from '@toast-ui/vue-editor';


export default {
    nama : 'industrial',
    data() {
        return {
            editorText : '',
            fee : '',
            period : '',
            rate : '',
            restriction : '',
            maximumReward : '',

            businessNo : '',
            safeGrade : '',
            workplaceType : '',
            workplacePosition : '',
            workplaceAddress : '',
        }
    },
    components : {
        Editor
    },
    computed: {
        employeeData : function(){
            return this.$store.getters.get_employee_data;
        },
    },
    methods: {
        async save(){
            this.editorText = this.getContent();
            try {
                await this.$store.dispatch('add_industrial', {
                    dtype : "industrial",
                    fee : this.fee,
                    rate : this.rate,
                    period : this.period,
                    maximumReward : this.maximumReward,
                    restriction : this.restriction,
                    content : this.editorText, // 여기까지 공통되는 insurance
                    businessNo : this.businessNo,
                    safeGrade : this.safeGrade,
                    workplaceType : this.workplaceType,
                    workplacePosition : this.workplacePosition,
                    workplaceAddress : this.workplaceAddress,
                    employeeIdx : this.employeeData.employeeIdx
                })
            }catch(err){
                console.log(err)
            }
        },
        getContent() {
          return this.$refs.toastEditor.invoke('getMarkdown')
        },
        goIdeaPage(){
            let result = confirm("취소하시겠습니까 ?");
            if(result){
                location.href='#/home'
            }
            
        },
    },
}
</script>