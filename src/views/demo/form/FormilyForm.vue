<template>
  <div>
    <Form :form="form">
      <SchemaField :schema="schema" />
    </Form>
  </div>
</template>

<script setup lang="ts">
  import { createForm } from '@formily/core';
  import { createSchemaField } from '@formily/vue';
  import { Form, FormItem, Input, Password } from '@formily/antdv';

  const { SchemaField } = createSchemaField({
    components: {
      Input,
      Password,
      FormItem,
    },
  });
  const form = createForm();
  const schema = {
    type: 'object',
    properties: {
      username: {
        type: 'string',
        title: '用户名',
        required: true,
        'x-decorator': 'FormItem',
        'x-component': 'Input',
      },
      email: {
        type: 'string',
        title: '邮箱',
        required: true,
        'x-validator': 'email',
        'x-decorator': 'FormItem',
        'x-component': 'Input',
      },
      oldPassword: {
        type: 'string',
        title: '原始密码',
        required: true,
        'x-decorator': 'FormItem',
        'x-component': 'Password',
      },
      password: {
        type: 'string',
        title: '新密码',
        required: true,
        'x-decorator': 'FormItem',
        'x-component': 'Password',
        'x-component-props': {
          checkStrength: true,
        },
        'x-reactions': [
          {
            dependencies: ['.confirm_password'],
            fulfill: {
              state: {
                selfErrors:
                  '{{$deps[0] && $self.value && $self.value !== $deps[0] ? "确认密码不匹配" : ""}}',
              },
            },
          },
        ],
      },
      confirm_password: {
        type: 'string',
        title: '确认密码',
        required: true,
        'x-decorator': 'FormItem',
        'x-component': 'Password',
        'x-component-props': {
          checkStrength: true,
        },
        'x-reactions': [
          {
            dependencies: ['.password'],
            fulfill: {
              state: {
                selfErrors:
                  '{{$deps[0] && $self.value && $self.value !== $deps[0] ? "确认密码不匹配" : ""}}',
              },
            },
          },
        ],
      },
    },
  };
</script>
